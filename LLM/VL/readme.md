from pydantic import BaseModel, Field
from typing import List, Literal
from langchain_core.output_parsers import JsonOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_community.llms import VLLM


# ================
# Pydantic схема
# ================

class SentimentAnalysis(BaseModel):
    sentiment: Literal["positive", "negative", "neutral"] = Field(
        description="Тональность отзыва: положительная, отрицательная или нейтральная"
    )
    confidence: float = Field(
        description="Уверенность в анализе от 0.0 до 1.0",
        ge=0.0, le=1.0
    )
    key_topics: List[str] = Field(
        description="Ключевые темы, упомянутые в отзыве",
        max_items=5
    )
    summary: str = Field(
        description="Краткое резюме отзыва в одном предложении",
        max_length=200
    )


# Парсер JSON
parser = JsonOutputParser(pydantic_object=SentimentAnalysis)


# ===========================
# Шаблон промпта
# ===========================

prompt_template = PromptTemplate(
    template="""Проанализируй отзыв: {review}

{format_instructions}

Ответь строго в формате JSON.
""",
    input_variables=["review"],
    partial_variables={
        "format_instructions": parser.get_format_instructions()
    }
)


# ===========================
# Подключаем Qwen-VL
# ===========================

# Напр.:
#   Qwen/Qwen2-VL-7B-Instruct
#   Qwen/Qwen2-VL-2B-Instruct
#   Qwen/Qwen2-VL-72B-Instruct
#   Qwen/Qwen-VL-Chat
model_path = "Qwen/Qwen3-VL-7B-Instruct"


llm = VLLM(
    model=model_path,
    trust_remote_code=True,     # обязательно для Qwen-VL
    temperature=0.0,            # детерминированный вывод
    vllm_kwargs={
        "max_model_len": 4096
    }
)


# Теперь можно запускать цепочку
#chain = prompt_template | llm | parser

# Пример вызова:
result = chain.invoke({"review": "Отличный товар, быстрая доставка!"})
print(result)норм?\