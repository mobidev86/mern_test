# MERN Practical Assignment (2 Hours)

## Member Descendant Tree + Ancestor Lookup (Up to 50 Levels)

### **Time Limit**
⏱ **2 Hours**

### **Stack Requirement**
- **Backend:** Node.js + Express + MongoDB (Mongoose)
- **Frontend:** React (any UI library allowed)

AI tools are allowed, but the final output must work correctly.

---

# 1. Objective
Build a small MERN application that manages a **member hierarchy tree** (parent-child relationship).

Using a **member ID**, the system must be able to return:
- **All ancestors** of the member (up to root)
- **All descendants** of the member (nested tree format)
- Support hierarchy depth up to **50 levels**

---

# 2. Data Model

## MongoDB Collection: `members`
Each member represents a node in a tree.

### Required fields:
```js
{
  name: String (required),
  parentId: ObjectId | null,
  createdAt: Date,
  updatedAt: Date
}
```

### Rules:
- Root members have `parentId = null`
- A member can have multiple children
- Maximum supported depth: **50**
- Must prevent invalid/circular relationships

---

# 3. Backend Requirements (Node.js + Express)

## 3.1 API Endpoints (Mandatory)

### ✅ 1) Create Member
**POST** `/api/members`

Body:
```json
{
  "name": "John Doe",
  "parentId": "optional-parent-id"
}
```

Rules:
- `name` is required (min 2 characters)
- If `parentId` is provided:
  - parent must exist
  - depth must not exceed 50
- Must reject invalid ObjectId
- Must reject circular relationship

Response example:
```json
{
  "message": "Member created successfully",
  "member": {
    "_id": "...",
    "name": "...",
    "parentId": "..."
  }
}
```

---

### ✅ 2) Get Member Details
**GET** `/api/members/:id`

Response:
```json
{
  "member": {
    "_id": "...",
    "name": "...",
    "parentId": "..."
  }
}
```

---

### ✅ 3) Get Ancestors (Up to 50 Levels)
**GET** `/api/members/:id/ancestors`

Must return ordered ancestors (**parent → root**).

Response:
```json
{
  "ancestors": [
    { "id": "...", "name": "...", "level": 1 },
    { "id": "...", "name": "...", "level": 2 }
  ]
}
```

Where:
- `level = 1` means direct parent
- Higher level means higher ancestor

---

### ✅ 4) Get Descendants (Nested Tree Format)
**GET** `/api/members/:id/descendants`

Must return nested JSON structure.

Response:
```json
{
  "member": { "id": "...", "name": "John" },
  "descendants": [
    {
      "id": "...",
      "name": "Child 1",
      "children": [
        {
          "id": "...",
          "name": "Grandchild",
          "children": []
        }
      ]
    }
  ]
}
```

Rules:
- Must support deep nesting up to 50 levels
- Must not crash due to recursion/infinite loop

---

### ✅ 5) Search Members
**GET** `/api/members?search=abc`

Returns list of matching members (by name).

Response:
```json
{
  "members": [
    { "_id": "...", "name": "...", "parentId": "..." }
  ]
}
```

---

## 3.2 Mandatory Validations
Candidate must implement the following validations.

Must reject:
- invalid Mongo ObjectId
- parentId not found
- self-parenting (`member.parentId == member._id`)
- circular reference (example: A → B → C → A)
- creating member beyond 50 levels depth

---

## 3.3 Performance Expectation
- Create MongoDB index on `parentId`
- Avoid full collection scan for every request

---

# 4. Frontend Requirements (React)

## 4.1 UI Pages / Screens

### ✅ Page 1: Create Member
Must include:
- Name input
- Parent selection dropdown (optional)
- Submit button
- Success/error message

---

### ✅ Page 2: Member Lookup
Must include:
- Input field to enter Member ID
- Buttons:
  - **Get Ancestors**
  - **Get Descendants**
- Display results clearly

---

## 4.2 Output Display Requirements

### Ancestors display format (example):
```text
Root
  -> Parent
      -> Current Member
```

### Descendants display format:
Nested indentation is enough, example:
```text
John
  - Child 1
      - Grandchild 1
  - Child 2
```

---

# 5. Constraints
- Max tree depth: **50**
- Candidate can use any React styling method
- Candidate can use any folder structure
- AI tools allowed, but the system must work fully

---

# 6. Deliverables (Must Submit)

## Backend Deliverables
1. Express backend project
2. MongoDB Member model implemented
3. Index on `parentId`
4. All required APIs implemented and working:
   - create member
   - get member
   - get ancestors
   - get descendants
   - search members
5. Validations implemented (cycle prevention + max depth 50)
6. Proper error responses + status codes

---

## Frontend Deliverables
7. React app with:
   - Create Member UI
   - Member Lookup UI
   - Ancestor visualization
   - Descendant tree visualization
8. Proper API integration and error handling

---

## Submission Deliverables
9. GitHub repository link OR zipped project folder
10. `README.md` containing:
   - Setup instructions for backend and frontend
   - `.env` example
   - Commands to run project
   - API endpoints list
11. Postman collection OR curl examples

---

# 7. Auto-Fail Conditions (Strict)
Candidate fails immediately if any of the following is true:

❌ Ancestor API does not return correct ordered list  
❌ Descendant API does not return nested tree format  
❌ Depth limit (50) is not enforced  
❌ Circular relationship is possible  
❌ Backend crashes on deep trees  
❌ Frontend does not display results  
❌ Project does not run with simple setup  
❌ No README submitted  

---

# 8. Bonus (Optional - Not Required)
If time allows, implement:

## Bonus Endpoint: Reparent Member
**PATCH** `/api/members/:id/reparent`

Body:
```json
{
  "newParentId": "some-id-or-null"
}
```

Rules:
- Must prevent cycle
- Must enforce max depth 50

---

# 9. Evaluation Criteria (What We Will Judge)
- Correctness of tree logic (ancestors/descendants)
- Handling edge cases
- Clean API design
- Proper validations
- Code readability and structure
- Ability to complete within time

---

## End of Assignment
