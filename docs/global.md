# Global Rules

- when I say “BE”, it refer to folder be-kindergarten-mgmt for backend services
- when I say “FE”, it refer to folder fe-kindergarten-mgmt for backend services

# Global Backend Rules

- EVERY endpoint or query - mutation graphql creation, SHOULD HAVE tested without finding Error.
- Every Domain SHOULD HAVE endpoint or Query - mutation for Create, Update, Delete by Id, Delete Multiple Id, Get One by Id, Get List All, Get List Pagination . For Example,Register Teacher, It will also create :
    1. Create Teacher
    2. Update Teacher
    3. Delete Teacher By Id
    4. Delete Multiple Teacher
    5. Get Teacher By Id
    6. Get List Teacher All
    7. Get List Teacher Pagination

- Example format Request and Response Body Below :

### **Teacher API Request & Response Body**

**1. Create Teacher**

**Request Body**

{

"full_name": "John Doe",

"email": "john.doe@example.com",

"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg"

}

**Success Response**

{

"success": true,

"message": "Teacher created successfully",

"data": {

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"full_name": "John Doe",

"email": "john.doe@example.com",

"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"

}

}

#### **2. Update Teacher**

**Request Body**

{

"full_name": "John Doe Updated",

"phone_number": "+6289876543210",

"address": "Jl. Asia Afrika No. 20, Bandung",

"position": "Senior Teacher",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe-updated.jpg"

}

**Success Response**

{

"success": true,

"message": "Teacher updated successfully",

"data": {

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"full_name": "John Doe Updated",

"email": "john.doe@example.com",

"phone_number": "+6289876543210",

"address": "Jl. Asia Afrika No. 20, Bandung",

"position": "Senior Teacher",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe-updated.jpg",

"updated_at": "2026-05-24T09:00:00Z"

}

}

#### **3. Delete Teacher By Id**

**Request Path**

DELETE /api/v1/teachers/{id}

**Success Response**

{

"success": true,

"message": "Teacher deleted successfully",

"data": {

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3"

}

}

#### **4. Delete Multiple Teacher**

**Request Body**

{

"ids": [

"01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"01JW4KR8Z1N9X4Y5U7P2D6E8F9",

"01JW4KS9A2B3C4D5E6F7G8H9J0"

]

}

**Success Response**

{

"success": true,

"message": "Teachers deleted successfully",

"data": {

"deleted_count": 3,

"deleted_ids": [

"01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"01JW4KR8Z1N9X4Y5U7P2D6E8F9",

"01JW4KS9A2B3C4D5E6F7G8H9J0"

]

}

}

#### **5. Get Teacher By Id**

**Request Path**

GET /api/v1/teachers/{id}

**Success Response**

{

"success": true,

"message": "Teacher retrieved successfully",

"data": {

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"full_name": "John Doe",

"email": "john.doe@example.com",

"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"

}

}

#### **6. Get List Teacher All**

**Request Path**

GET /api/v1/teachers/all

**Success Response**

{

"success": true,

"message": "Teachers retrieved successfully",

"data": [

{

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"full_name": "John Doe",

"email": "john.doe@example.com",

"position": "Homeroom Teacher",


"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"


},

{

"id": "01JW4KR8Z1N9X4Y5U7P2D6E8F9",

"full_name": "Jane Smith",

"email": "jane.smith@example.com",

"position": "Assistant Teacher",


"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"


}

]

}

#### **7. Get List Teacher Pagination**

**Request Query**

GET /api/v1/teachers?page=1&limit=10&search=john&status=active&sort_by=created_at&sort_order=desc

**Success Response**

{

"success": true,

"message": "Teachers retrieved successfully",

"data": {

"items": [

{

"id": "01JW4KQ7Y9E8V2F3T6M8A1B2C3",

"full_name": "John Doe",

"email": "john.doe@example.com",

"phone_number": "+6281234567890",

"position": "Homeroom Teacher",


"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"


},

{

"id": "01JW4KR8Z1N9X4Y5U7P2D6E8F9",

"full_name": "Jane Smith",

"email": "jane.smith@example.com",

"phone_number": "+6281122233344",

"position": "Assistant Teacher",


"phone_number": "+6281234567890",

"gender": "male",

"birth_place": "Bandung",

"birth_date": "1990-05-10",

"address": "Jl. Merdeka No. 10, Bandung",

"religion": "Islam",

"nationality": "Indonesia",

"marital_status": "married",

"employee_number": "TCH-2026-0001",

"join_date": "2026-05-24",

"position": "Homeroom Teacher",

"education_level": "Bachelor",

"major": "Early Childhood Education",

"status": "active",

"photo_url": "https://cdn.example.com/teachers/john-doe.jpg",

"created_at": "2026-05-24T08:00:00Z",

"updated_at": "2026-05-24T08:00:00Z"


}

],

"pagination": {

"page": 1,

"limit": 10,

"total_items": 25,

"total_pages": 3,

"has_next_page": true,

"has_previous_page": false

}

}

}

# Global Front end Rules

## State Management

- TanStack Query for API state
- TanStack Store for global UI state
- local state for forms only

## Components

- reusable first
- Use component from solid-ui first
    - accessibility required
- mobile-first

## API

- never call fetch directly
- use centralized API client