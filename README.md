# React + React Query + TypeScript + Supabase

This is a hotel booking web application built with React, React Query, TypeScript, and Supabase.

## 🚀 Getting Started Locally

1. Clone the repository.
2. Create a `.env.local` file using `.env.local.example` as a template, and add your Supabase credentials.
3. Run the development server:

   ```bash
   npm install
   npm run dev
   ```

## Live Demo
- The project is hosted on Vercel.
- Try it out here: https://quannb96-cabin-booking-vite-react.vercel.app/signup

##  Supabase Setup

- If you don’t have an account yet, create one at https://supabase.com/.

- Create a new project.

- Use the following SQL to set up the required tables:

## SQL Schema for Supabase

``` sql
-- Guests table
create table public.guests (
  id bigint generated always as identity primary key,
  full_name text not null,
  email text not null unique,
  nationality text,
  national_id text,
  country_flag text
);

-- Cabins table
create table public.cabins (
  id bigint generated always as identity primary key,
  name text not null unique,
  max_capacity integer not null,
  regular_price numeric not null,
  discount numeric default 0,
  image text,
  description text
);

-- Bookings table
create table public.bookings (
  id bigint generated always as identity primary key,
  created_at timestamptz default now(),
  start_date date not null,
  end_date date not null,
  cabin_id bigint references cabins(id) on delete cascade,
  guest_id bigint references guests(id) on delete cascade,
  has_breakfast boolean default false,
  observations text,
  is_paid boolean default false,
  num_guests integer not null,
  num_nights integer,
  cabin_price numeric,
  extras_price numeric,
  total_price numeric,
  status text check (status in ('unconfirmed', 'checked-in', 'checked-out')) default 'unconfirmed'
);

-- Settings table
create table public.settings (
  id bigint generated always as identity primary key,
  min_booking_length integer not null default 1,
  max_booking_length integer not null default 30,
  max_guests_per_booking integer not null default 10,
  breakfast_price numeric not null default 15
);

-- Initial settings record
insert into public.settings (
  min_booking_length,
  max_booking_length,
  max_guests_per_booking,
  breakfast_price
) values (3, 30, 10, 15);
```
