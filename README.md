# 📚 Schoolly

Додаток для управління домашніми завданнями та розкладом.

## Стек
- React + Vite
- Tailwind CSS
- Supabase (база даних + авторизація)

## Налаштування

### 1. Встанови залежності
```bash
npm install
```

### 2. Налаштуй Supabase
1. Зареєструйся на [supabase.com](https://supabase.com)
2. Створи новий проект
3. Скопируй `.env.example` → `.env`
4. Встав свої ключі з Settings → API

```env
VITE_SUPABASE_URL=https://xxxxxxxxxx.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here
```

### 3. Створи таблиці в Supabase

Відкрий **SQL Editor** в Supabase і виконай:

```sql
-- Таблиця домашніх завдань
create table homeworks (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  subject text not null,
  description text not null,
  due_date date,
  completed boolean default false,
  created_at timestamp with time zone default now()
);

-- Таблиця розкладу
create table schedule (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  day text not null,
  lesson_number integer not null,
  subject text not null,
  time_start time,
  room text,
  created_at timestamp with time zone default now()
);

-- Права доступу (кожен бачить тільки своє)
alter table homeworks enable row level security;
alter table schedule enable row level security;

create policy "Users can manage own homeworks" on homeworks
  for all using (auth.uid() = user_id);

create policy "Users can manage own schedule" on schedule
  for all using (auth.uid() = user_id);
```

### 4. Запусти проект
```bash
npm run dev
```

## Деплой на Vercel
1. Залий на GitHub
2. Підключи репо на [vercel.com](https://vercel.com)
3. Додай змінні середовища (VITE_SUPABASE_URL, VITE_SUPABASE_ANON_KEY)
4. Deploy!
