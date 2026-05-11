# Book Library Management System

A complete Laravel 11 CRUD application for managing a book library with a dark, elegant UI using Blade templates and Tailwind CSS.

## Features

### ✅ Implemented Features

- **Complete CRUD Operations**: Create, Read, Update, Delete books
- **Advanced Search & Filtering**: Search by title/author and filter by genre
- **Dashboard**: Statistics dashboard with:
  - Total books count
  - Available books count
  - Total genres count
  - Low stock warnings (< 5 items)
  - Recent books table
  - Books by genre chart
- **Book Management**:
  - Cover image upload with live preview
  - Multiple genre selection (Fiction, Non-Fiction, Science, History, Technology, Biography, Other)
  - ISBN tracking with uniqueness validation
  - Stock management
  - Availability status toggle
  - Soft deletes
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Dark Theme**: Elegant dark UI with indigo/violet accents using Tailwind CSS
- **Related Books**: Shows books from the same genre on detail page
- **Flash Messages**: Auto-dismissing success/error notifications
- **Pagination**: 10 books per page
- **Form Validation**: Comprehensive server-side validation with error messages

## Tech Stack

- **Framework**: Laravel 11
- **Database**: SQLite (default, easily configurable to MySQL)
- **Frontend**: Blade Templates + Tailwind CSS 4.0
- **JavaScript**: Vanilla JS for image preview
- **Authentication**: Built-in Laravel authentication ready

## Installation & Setup

### Prerequisites

- PHP 8.3+
- Composer
- Node.js & npm

### Quick Start

```bash
# 1. Navigate to the project directory
cd "c:\laragon\www\Book Library Management System\book-library"

# 2. Install PHP dependencies (already done)
composer install

# 3. Install Node dependencies (already done)
npm install

# 4. Build frontend assets (already done)
npm run build

# 5. Run migrations with seeder (already done)
php artisan migrate:fresh --seed --seeder=BookSeeder

# 6. Create storage symlink (already done)
php artisan storage:link

# 7. Start development server
php artisan serve --port=8000

# 8. Access the application
# Open http://127.0.0.1:8000 in your browser
```

## Default Routes

- **Dashboard**: `/dashboard` - Statistics and recent books
- **Books List**: `/books` - View all books with search/filter
- **Create Book**: `/books/create` - Add a new book
- **View Book**: `/books/{id}` - Book details and related books
- **Edit Book**: `/books/{id}/edit` - Modify book information
- **Delete Book**: `/books/{id}` - Remove a book (POST/DELETE)

## Database Structure

### Books Table

```sql
- id (Primary Key)
- title (String, Required)
- author (String, Required)
- genre (Enum: Fiction, Non-Fiction, Science, History, Technology, Biography, Other)
- isbn (String, Unique, Nullable)
- published_year (Year, Nullable)
- price (Decimal 8,2, Nullable) - in PKR
- stock (Integer, Default: 0)
- description (Text, Nullable)
- cover_image (String, Nullable)
- is_available (Boolean, Default: true)
- deleted_at (Timestamp, Nullable) - for soft deletes
- created_at, updated_at (Timestamps)
```

## File Structure

```
app/
├── Http/
│   ├── Controllers/
│   │   └── BookController.php (All CRUD + Dashboard logic)
│   └── Requests/
│       └── BookRequest.php (Form validation)
├── Models/
│   └── Book.php (Model with scopes and accessors)

database/
├── migrations/
│   └── create_books_table.php
└── seeders/
    └── BookSeeder.php (15 sample books)

resources/
└── views/
    ├── layouts/
    │   └── app.blade.php (Main layout with sidebar)
    └── books/
        ├── index.blade.php (Books grid with search/filter)
        ├── create.blade.php (Add new book form)
        ├── edit.blade.php (Edit book form)
        ├── show.blade.php (Book details with related books)
        └── dashboard.blade.php (Statistics dashboard)

routes/
└── web.php (Book resource routes)
```

## Key Features Explained

### 1. Book Model (`app/Models/Book.php`)

- **Fillable Fields**: All columns except timestamps
- **Soft Deletes**: Books can be soft-deleted without permanent removal
- **Accessor**: `getAvailabilityBadgeAttribute()` - Returns 'Available' or 'Out of Stock'
- **Scopes**: 
  - `scopeAvailable()` - Filter available books
  - `scopeByGenre($genre)` - Filter by genre

### 2. BookController (`app/Http/Controllers/BookController.php`)

- **index()**: List books with pagination (10 per page), search by title/author, filter by genre
- **create()**: Return form with genres list
- **store()**: Validate and save book with image upload to `storage/public/books`
- **show()**: Display book details with 4 related books from same genre
- **edit()**: Return edit form with current book data
- **update()**: Update book with new image handling (delete old, upload new)
- **destroy()**: Delete book and associated image
- **dashboard()**: Statistics and recent books view

### 3. BookRequest (`app/Http/Requests/BookRequest.php`)

Validation rules:
- `title`: Required, string, max 255
- `author`: Required, string, max 255
- `genre`: Required, must be one of the 7 genres
- `isbn`: Nullable, unique (ignores current on update)
- `published_year`: Nullable, min 1000, max current year
- `price`: Nullable, numeric, min 0
- `stock`: Nullable, integer, min 0
- `description`: Nullable, string, max 2000
- `cover_image`: Nullable, image (jpeg, png, jpg, webp), max 2MB
- `is_available`: Nullable, boolean

### 4. Views

All views use:
- **Tailwind CSS** for styling
- **Dark theme** (slate-800/900 backgrounds)
- **Indigo/purple accents** for interactive elements
- **Responsive grid layout** (3 cols desktop, 2 tablet, 1 mobile)
- **Form validation errors** displayed inline
- **Flash messages** auto-dismiss after 3 seconds

## Sample Data

The BookSeeder includes 15 books across all genres:
- **Fiction** (5 books): The Great Gatsby, To Kill a Mockingbird, 1984, The Midnight Library, Pride and Prejudice
- **Science** (3 books): A Brief History of Time, The Selfish Gene, Cosmos
- **Non-Fiction** (2 books): Sapiens, The Art of War
- **Technology** (2 books): The Innovators, The Lean Startup
- **Biography** (2 books): The Code Breaker, Educated
- **History** (1 book): The Crusades

## API Endpoints

All standard RESTful routes are available:

```
GET    /books              - List all books (with pagination)
GET    /books/create       - Show create form
POST   /books              - Store new book
GET    /books/{id}         - Show book details
GET    /books/{id}/edit    - Show edit form
PUT    /books/{id}         - Update book
DELETE /books/{id}         - Delete book
GET    /dashboard          - Show dashboard
```

## Image Upload

- Uploaded images are stored in `storage/public/books/`
- Public access via `/storage/books/filename`
- Supported formats: JPEG, PNG, JPG, WebP
- Max file size: 2MB
- Old images are deleted when replacing

## Pagination

- 10 books per page
- Uses Laravel's default Tailwind pagination
- Works with search and filter parameters

## Search & Filter

- **Search**: Searches in book title and author name (case-insensitive)
- **Genre Filter**: Filters books by single genre
- **Combined**: Both can be used together
- **Clear Filters**: Button to reset both filters

## Styling

- **Colors**: Dark slate backgrounds (#0f172a, #1e293b), indigo accents (#4f46e5)
- **Typography**: Clean, readable fonts
- **Spacing**: Consistent padding and margins using Tailwind scale
- **Hover States**: Interactive elements have hover effects
- **Gradients**: Used for covers and stat cards

## Configuration

### Database (SQLite - Default)

The application uses SQLite by default for ease of development. To switch to MySQL:

1. Update `.env`:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=book_library
DB_USERNAME=root
DB_PASSWORD=
```

2. Create database:
```bash
mysql -u root
CREATE DATABASE book_library;
EXIT;
```

3. Run migrations:
```bash
php artisan migrate:fresh --seed --seeder=BookSeeder
```

### File Storage

The storage symlink allows public access to uploaded images. If you need to recreate it:

```bash
php artisan storage:link
```

## Performance

- **Pagination**: Limits query results to 10 per page
- **Database Indexes**: Primary key and unique constraints on ISBN
- **Soft Deletes**: Queries exclude deleted records by default
- **Image Optimization**: Supports modern formats (WebP)

## Security

- **CSRF Protection**: All forms include CSRF tokens
- **Validation**: Server-side validation on all inputs
- **Authorization**: Routes are ready for authentication
- **SQL Injection**: Protected via Eloquent ORM

## Troubleshooting

### Port Already in Use
```bash
php artisan serve --port=8001
```

### Database Lock
```bash
php artisan migrate:fresh --seed --seeder=BookSeeder
```

### Build Issues
```bash
npm install
npm run build
```

### Storage Link Issues
```bash
php artisan storage:link --force
```

## Development

### Watch Mode (Development with Hot Reload)
```bash
npm run dev
```

In another terminal:
```bash
php artisan serve
```

### Production Build
```bash
npm run build
php artisan optimize
php artisan config:cache
```

## License

This is a demonstration project for educational purposes.

## Agentic Development

Laravel's predictable structure and conventions make it ideal for AI coding agents like Claude Code, Cursor, and GitHub Copilot. Install [Laravel Boost](https://laravel.com/docs/ai) to supercharge your AI workflow:

```bash
composer require laravel/boost --dev

php artisan boost:install
```

Boost provides your agent 15+ tools and skills that help agents build Laravel applications while following best practices.

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
