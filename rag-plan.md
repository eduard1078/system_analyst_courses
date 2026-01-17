# ПРОГРАММА КУРСА: "RAG и ИИ: Система поиска и генерации"
## Для системных аналитиков | Deep Learning подход

---

## PET-ПРОЕКТ КУРСА:
**"Enterprise Search Engine — AI-поиск по внутренней документации компании"**

Во время курса студенты разработают полнофункциональный RAG систему:
- Загрузка документов (PDF, Word, Markdown, JSON)
- Vectorization через embedding модели (Yandex Embeddings API, local)
- Vector database (Qdrant, Milvus, или FAISS для простоты)
- Semantic search с ranking
- LLM-powered ответы с ссылками на источники
- Web интерфейс для поиска
- Analytics: популярные запросы, quality metrics

---

## СТРУКТУРА КУРСА "RAG и ИИ: Система поиска и генерации"

### МОДУЛЬ 1: Теория RAG и Vector Embeddings (90 мин.)
**Занятие 1.1: Проблема традиционного поиска** (15 мин.)
- Keyword search: ограничения (синонимы, контекст, семантика)
- Пример: "Как оплатить счёт?" vs "Способы денежных расчётов"
- Нужна семантика, а не точные слова

**Занятие 1.2: Что такое embeddings (векторные представления)?** (20 мин.)
- Слово/предложение → вектор в n-мерном пространстве
- Похожие слова → близкие векторы (cosine distance)
- Примеры: king - man + woman = queen
- Как обучаются embeddings (не нужно учить deep learning)

**Занятие 1.3: Архитектура RAG системы** (20 мин.)
- Шаг 1: Indexing (документы → chunks → embeddings → vector DB)
- Шаг 2: Retrieval (запрос → embedding → поиск в vector DB)
- Шаг 3: Generation (LLM: контекст + запрос → ответ)
- Vs Google Search: RAG фокусится на приватных данных + accuracy

**Занятие 1.4: Реальные применения RAG** (15 мин.)
- Customer support: быстрые ответы на FAQ
- Internal knowledge: поиск по внутренней документации
- Medical diagnosis: поиск по медицинским статьям
- Legal: анализ контрактов и прецендентов
- Научные работы: поиск по публикациям

**Занятие 1.5: Подготовка окружения разработки** (20 мин.)
- Установка Python 3.10+ и настройка виртуального окружения
- Установка Qdrant (Docker: `docker run -p 6333:6333 qdrant/qdrant`)
- Альтернатива: FAISS для локальной разработки (`pip install faiss-cpu`)
- Yandex Embeddings API: получение ключа для векторизации
- Установка Cursor IDE для AI-assisted разработки
- Структура проекта: indexer, retriever, generator, api

---

### МОДУЛЬ 2: Embeddings и Vector Databases (70 мин.)
**Занятие 2.1: Выбор embedding модели** (20 мин.)
- **Yandex Embeddings API** (облако, русский язык, быстро)
- **OpenAI Embeddings** (лучше всех, но дорого)
- **Local embeddings** (sentence-transformers, privacy)
- **Multilingual models** (для многоязычных данных)
- Сравнение: качество vs стоимость vs скорость

**Занятие 2.2: Создание vector database (Qdrant)** (25 мин.)
- Установка Qdrant (локально или облако)
- Создание collection (имя, размер вектора)
- Добавление points (вектор + метаданные)
- Поиск: similarity search

**Занятие 2.3: Альтернативы: FAISS, Milvus, Pinecone** (15 мин.)
- **FAISS** (Facebook): fast, local, best для 1M+ документов
- **Milvus** (open-source Pinecone): scalable, distributed
- **Pinecone** (SaaS): простой, но дорогой
- Выбор для курса: Qdrant (хороший баланс)

**Занятие 2.4: Indexing большого количества документов** (10 мин.)
- Batch processing: индексировать по 100 документов
- Progress tracking и error handling
- Оптимизация: параллельные embeddings

---

### МОДУЛЬ 3: Подготовка документов (Document Processing) (65 мин.)
**Занятие 3.1: Загрузка различных форматов** (15 мин.)
- **PDF**: PyPDF2, pdfplumber, PDF processing
- **Word**: python-docx
- **Markdown**: простой парсинг
- **JSON**: parsing структурированных данных
- **Web pages**: BeautifulSoup, Selenium

**Занятие 3.2: Chunking и разбиение документов** (20 мин.)
- Проблема: embeddings имеют limit на длину (обычно 512-8K tokens)
- Решение: разбить документ на chunks (параграфы, предложения)
- Стратегии: fixed size, semantic chunks, sliding window

**Занятие 3.3: Метаданные и структурирование** (15 мин.)
- Хранение metadata: источник, дата, автор, раздел
- Metadata filtering: "показать только свежие документы"
- Иерархия: документ → раздел → параграф → chunk

**Занятие 3.4: Cleaning и нормализация текста** (15 мин.)
- Удаление HTML тегов, специальных символов
- Нормализация пробелов и переводов строк
- Language detection и handling многоязычных текстов
- Duplicate detection

---

### МОДУЛЬ 4: Retrieval (Поиск релевантных документов) (60 мин.)
**Занятие 4.1: Similarity search и ranking** (20 мин.)
- Cosine similarity: основной метрик для поиска
- Top-K retrieval: возвращаем топ 5-10 результатов
- Threshold-based filtering: только выше 0.7 similarity
- Re-ranking: улучшение порядка результатов

**Занятие 4.2: Hybrid search (keyword + semantic)** (15 мин.)
- Только semantic: может пропустить точные ключевые слова
- Только keyword: не понимает синонимы и контекст
- Комбо: BM25 (keyword) + vector search (semantic) + fusion
- Пример: Elasticsearch для keyword, Qdrant для vectors

**Занятие 4.3: Query expansion и reformulation** (15 мин.)
- Пользователь пишет короткий запрос: "оплата"
- LLM расширяет: "способы оплаты счёта, методы платежа, расчёты"
- Поиск по расширенному запросу → лучше результаты
- Multi-query retrieval

**Занятие 4.4: Context ranking и relevance scoring** (10 мин.)
- Не все релевантные документы одинаково важны
- Ранжирование по: freshness, authority, user engagement
- Learning-to-rank (продвинуто): модель для ранжирования

---

### МОДУЛЬ 5: Generation — LLM ответы с контекстом (60 мин.)
**Занятие 5.1: Prompt construction для RAG** (20 мин.)
- Шаблон: "Контекст: [документы]. Вопрос: [query]. Ответ:"
- Добавление source attribution: "Согласно документу X..."
- Few-shot примеры в prompt для улучшения качества
- Балансирование: краткость vs полнота

**Занятие 5.2: Handling hallucinations (когда LLM выдумывает)** (15 мин.)
- Проблема: LLM может "вспомнить" информацию, которой нет в контексте
- Решение 1: Strict prompt ("ТОЛЬКО из контекста")
- Решение 2: Fact-checking: проверить ответ против источников
- Решение 3: Structured output: LLM возвращает JSON с ссылками

**Занятие 5.3: Streaming и progressive generation** (15 мин.)
- Долгие ответы нужны streaming (progressive output)
- Пользователь видит результат по мере генерации
- Web interface: WebSocket для streaming ответов

**Занятие 5.4: Multi-turn conversation с памятью** (10 мин.)
- Сохранение истории диалога: user → bot → user → bot
- Re-ranking на основе контекста предыдущих сообщений
- Coherence: новый ответ должен соответствовать диалогу

---

### МОДУЛЬ 6: RAG в production (70 мин.)
**Занятие 6.1: Yandex Embeddings API для production** (20 мин.)
- Масштабирование: batch embeddings API
- Rate limiting: максимум запросов в сек
- Кэширование embeddings: не пересчитывать одни и те же
- Cost optimization: правильно выбрать размер batch

**Занятие 6.2: FastAPI для RAG сервиса** (25 мин.)
- Эндпоинты: POST /search, GET /health, POST /feedback
- Авторизация: API keys, JWT tokens
- Rate limiting: на пользователя, на IP
- Caching: Redis для часто запрашиваемых вопросов

**Занятие 6.3: Admin panel и управление документами** (15 мин.)
- Загрузка новых документов
- Переиндексирование (если изменили embedding модель)
- Статистика: количество документов, запросов, популярные
- Delete/update документов

**Занятие 6.4: Docker и deployment** (10 мин.)
- Docker для: FastAPI app, Qdrant, embedding service
- docker-compose для локальной разработки
- CI/CD: GitHub Actions для автоматического deployment

---

### МОДУЛЬ 7: Evaluation и качество RAG (55 мин.)
**Занятие 7.1: Метрики качества RAG** (20 мин.)
- **Retrieval metrics:**
  - Recall@K: Сколько релевантных документов в топ-K?
  - Precision@K: Сколько из топ-K действительно релевантны?
  - MRR (Mean Reciprocal Rank): на какой позиции первый релевантный?
  - NDCG (Normalized Discounted Cumulative Gain)

- **Generation metrics:**
  - BLEU, ROUGE: сравнение с эталонным ответом
  - Human evaluation: пользователи оценивают (1-5 звёзд)
  - Factuality: ответ соответствует источникам?

**Занятие 7.2: Создание тестового набора (benchmark)** (15 мин.)
- Собрание 50-100 типичных вопросов
- Для каждого: эталонный ответ + релевантные документы
- Автоматическое тестирование: запустили и сравнили
- Регрессионное тестирование: новые изменения не сломали старое

**Занятие 7.3: A/B testing и улучшения** (15 мин.)
- Тестирование 2 вариантов: старый embedding vs новый
- Метрика: retrieval quality, user satisfaction
- Feedback loop: пользователи указывают, полезен ли ответ

**Занятие 7.4: Analytics и monitoring** (5 мин.)
- Логирование: какие запросы, какие ответы, feedback
- Visualizations: популярные вопросы, fail cases
- Alerts: если quality упал ниже threshold

---

### МОДУЛЬ 8: Advanced RAG techniques (60 мин.)
**Занятие 8.1: Prompt engineering для RAG** (15 мин.)
- Few-shot learning: примеры хороших ответов в prompt
- Instruction tuning: как коротко дать инструкции LLM
- Persona-based prompting: "Ты эксперт по IT услугам"
- Chain-of-thought: "Давай размышлять пошагово"

**Занятие 8.2: Самоулучшение: LLM переписывает документы** (20 мин.)
- Документы часто написаны плохо (неструктурировано, синтаксис ошибки)
- LLM может переписать для лучшего поиска
- Пример: Документ "Зачем использовать ИИ?" → улучшенная версия с структурой
- Риск: LLM может добавить неправильную информацию (нужен review)

**Занятие 8.3: Knowledge graphs + RAG** (15 мин.)
- Кроме текста: хранить факты в виде графа (Entity → Relation → Entity)
- "Александр Петров → CEO → Компания А"
- Поиск в графе + текст = более точные ответы
- Пример: Neo4j для граф БД

**Занятие 8.4: Cross-encoder re-ranking** (10 мин.)
- Bi-encoder (FAISS search) быстрый, но не точный
- Cross-encoder (специальная модель) медленный, но точный
- Двухэтапный поиск: Bi-encoder top-100 → Cross-encoder top-5
- Балансирование: скорость vs точность

---

### МОДУЛЬ 9: Специализированные RAG (50 мин.)
**Занятие 9.1: RAG для различных типов данных** (15 мин.)
- **Табличные данные**: SQL + Text (таблица + описание)
- **Изображения**: OCR + embeddings для картинок
- **Видео**: транскрипция → RAG
- **Код**: GitHub, StackOverflow, документация

**Занятие 9.2: Multi-modal RAG** (15 mин)
- Документ содержит текст + изображения + таблицы
- Обработка изображений: OCR или CLIP (vision embeddings)
- Единая search по всем модальностям
- Пример: PowerPoint с текстом и диаграммами

**Занятие 9.3: RAG на русском языке** (15 min)
- Специфика русского: падежи, число, род
- Yandex Embeddings: оптимизирована для русского
- Нужна лемматизация? Да, для keyword matching
- Multilingual embeddings: когда нужна поддержка нескольких языков

**Занятие 9.4: Real-time RAG** (5 min)
- Данные обновляются постоянно (новости, метрики)
- Нужна live indexing: как только документ добавлен → в индекс
- Кэширование: invalidate cache при изменении

---

### МОДУЛЬ 10: RAG в реальных проектах (45 мин.)
**Занятие 10.1: Customer support RAG** (15 мин.)
- FAQ + документация + логи чатов
- Автоматические ответы для 80% вопросов
- Escalation: сложные вопросы → человеку
- Metrics: resolution rate, avg resolution time, CSAT

**Занятие 10.2: Internal knowledge base RAG** (15 мин.)
- Вместо Confluence: AI search по вики
- Быстрый доступ к информации (faster than Google Search на private data)
- Collaborative: лучше с Obsidian, Roam Research
- Use case: архитектурные решения, best practices, runbooks

**Занятие 10.3: Безопасность и приватность RAG** (15 мин.)
- Sensitive data: кредитные карты, персональные данные
- Masking: скрывать PII перед embedding
- Access control: какие пользователи видят какие документы
- GDPR compliance: удаление data на запрос