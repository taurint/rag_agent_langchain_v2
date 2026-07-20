# RAG Agent v2 🚀

RAG-агент на LangChain с продвинутыми техниками retrieval и production-мониторингом.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![LangChain](https://img.shields.io/badge/LangChain-0.3+-green)
![OpenAI](https://img.shields.io/badge/OpenAI-API-orange)
![FAISS](https://img.shields.io/badge/FAISS-vector--store-red)

## ✨ Что нового в v2

| Фича | v1 (базовая) | v2 (улучшенная) |
|---|---|---|
| **Retrieval** | Простой similarity search | + Реранкер (Cross-Encoder) |
| **Поиск** | Только семантический (dense) | + Гибридный (dense + sparse) |
| **Мониторинг** | Нет | Структурированное логирование + замер latency |
| **Оценка качества** | Keyword matching | + Сравнение latency трёх подходов |

## 🧠 Архитектура
ЗАПРОС ПОЛЬЗОВАТЕЛЯ
"Как вернуть товар?"

═══════════════════════════════════════════
RETRIEVAL — поиск релевантных чанков
═══════════════════════════════════════════

Dense поиск (FAISS)
Находит топ-6 чанков по смысловому сходству

Sparse поиск (TF-IDF)
Находит топ-6 чанков по ключевым словам

Hybrid Fusion
Смешивает результаты dense и sparse
Веса: 0.7 (dense) и 0.3 (sparse)

Cross-Encoder (реранкер)
Переоценивает пары "запрос + чанк"
Выбирает топ-3 самых релевантных чанка

═══════════════════════════════════════════
GENERATION — генерация ответа
═══════════════════════════════════════════

PromptTemplate
Системный промпт: "Ты ассистент поддержки. Отвечай только по контексту."
Контекст: топ-3 чанка
Вопрос: запрос пользователя

LLM (GPT-4o-mini, temperature=0)
Генерирует ответ строго на основе контекста

═══════════════════════════════════════════
ОТВЕТ ПОЛЬЗОВАТЕЛЮ
═══════════════════════════════════════════

"Для возврата нужен чек и паспорт. Срок возврата 14 дней."

═══════════════════════════════════════════
MONITORING — мониторинг и логирование
═══════════════════════════════════════════

Latency на каждом этапе
retrieval_ms — время поиска чанков
generation_ms — время генерации ответа
total_ms — общее время от запроса до ответа

Структурированные логи
request_started — начало обработки запроса
retrieval_completed — какие чанки найдены
request_completed — готовый ответ и метрики

Статистика запросов
total_requests — сколько всего запросов обработано
avg_latency_ms — среднее время ответа

## 📦 Стек

- **LangChain** — оркестрация цепочек
- **OpenAI** — GPT-4o-mini (генерация) + text-embedding-3-small (эмбеддинги)
- **FAISS** — векторная БД (dense retrieval)
- **TF-IDF** — sparse retrieval (ключевые слова)
- **Cross-Encoder** — реранкер (ms-marco-MiniLM-L-6-v2)
- **Sentence-Transformers** — эмбеддинги для реранкера

## 📊 Метрики (v2)
Метрика	Значение
Keyword Match Rate	77% (baseline)
Средняя latency (hybrid)	~2.6 сек
Retrieval (гибрид)	~700 мс
Generation (LLM)	~1900 мс

## 🔄 Версии
v2	Реранкер (Cross-Encoder), гибридный поиск (dense + sparse), structured logging, замер latency

## 👩‍💻 Автор
Наталья Чижкова — AI/LLM Engineer

GitHub: taurint
