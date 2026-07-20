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
Запрос пользователя
│
▼
┌─────────────────────────────────────┐
│ RETRIEVAL │
│ │
│ ┌──────────┐ ┌────────────────┐ │
│ │ Dense │ │ Sparse │ │
│ │ (FAISS) │ │ (TF-IDF) │ │
│ └────┬─────┘ └───────┬────────┘ │
│ │ │ │
│ └────────┬─────────┘ │
│ ▼ │
│ Hybrid Fusion │
│ (веса 0.7/0.3) │
│ │ │
│ ▼ │
│ ┌────────────────────┐ │
│ │ Cross-Encoder │ │
│ │ (Рера́нкер) │ │
│ └────────┬───────────┘ │
│ │ │
└─────────────┼──────────────────────┘
▼
Топ-3 релевантных чанка
│
▼
┌─────────────────────────────────────┐
│ GENERATION │
│ │
│ PromptTemplate + LLM (GPT-4o-mini)│
│ │
└─────────────────────────────────────┘
│
▼
Ответ пользователю
│
▼
┌─────────────────────────────────────┐
│ MONITORING │
│ │
│ • Latency на каждом этапе │
│ • Структурированные логи │
│ • Статистика запросов │
└─────────────────────────────────────┘

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
