```mermaid
erDiagram
    %% User Authentication & Profile System
    User {
        int id PK
        string identifier "Email (unique)"
        string password
        boolean is_active
        boolean is_staff
        boolean must_change_password
        datetime date_joined
        datetime updated_at
    }

    Profile {
        int id PK
        int user_id FK
        string first_name
        string middle_name
        string last_name
        string suffix
        text photo
        int role "1=Admin, 2=Mentor, 3=Student"
        int year_level "1-4"
        int term_level "1-3"
        date date_of_birth
        string gender
        string student_number
        string employee_number
        int department_id FK
        int specialization_id FK
        int current_school_year_id FK
        int current_term_id FK
        datetime date_joined
        datetime updated_at
    }

    %% Department & Academic Structure
    Department {
        int id PK
        string name
        string code "unique"
        text description
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    Specialization {
        int id PK
        string name
        int department_id FK
        text description
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    SchoolYear {
        int id PK
        int start_year
        int end_year
        string year_display "e.g., 2024-2025"
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    Term {
        int id PK
        int school_year_id FK
        string name
        date start_date
        date end_date
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    %% Course Management System
    Course {
        int id PK
        string title
        text description
        string code "unique"
        text image
        int created_by_id FK
        int department_id FK
        int specialization_id FK
        int school_year_id FK
        string term
        string year_level
        string section
        datetime start_date
        datetime end_date
        datetime created_at
        datetime updated_at
    }

    SectionCourse {
        int id PK
        int course_id FK
        string section
        datetime created_at
        datetime updated_at
    }

    CourseMentor {
        int id PK
        int course_id FK
        datetime created_at
        datetime updated_at
    }

    CourseMentor_mentors {
        int id PK
        int coursementor_id FK
        int user_id FK
    }

    CourseStudent {
        int id PK
        int course_id FK
        datetime created_at
        datetime updated_at
    }

    CourseStudent_students {
        int id PK
        int coursestudent_id FK
        int user_id FK
    }

    CoursePrerequisites {
        int id PK
        int course_id FK
        int prerequisite_id FK
        datetime created_at
        datetime updated_at
    }

    %% Course Content Structure
    Module {
        int id PK
        int course_id FK
        string title
        text description
        int week_number "1-12"
        int order_index
        boolean is_draft
        datetime start_date
        datetime end_date
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    Lesson {
        int id PK
        int module_id FK
        string title
        text description
        text content "Rich text from WYSIWYG"
        int duration "minutes"
        int order
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    LearningObjective {
        int id PK
        int lesson_id FK
        text objective
        int order
        datetime created_at
    }

    LessonMedia {
        int id PK
        int lesson_id FK
        string title
        string url
        string file_type "pdf, doc, video, etc."
        datetime created_at
    }

    LessonLink {
        int id PK
        int lesson_id FK
        string title
        string url
        text description
        datetime created_at
    }

    %% Assignments & Assessments
    Homework {
        int id PK
        int module_id FK
        string title
        text content
        decimal total_points
        datetime deadline
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    Milestone {
        int id PK
        int course_id FK
        string title
        text description
        int type "1=Milestone1, 2=Milestone2, 3=Terminal"
        int week_number "1-12"
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    %% Submission & Grading System
    Submission {
        int id PK
        text submission
        int student_id FK
        int homework_id FK "nullable"
        int milestone_id FK "nullable"
        decimal grade
        int status "1=Graded, 2=InReview, 3=Submitted, 4=NotSubmitted"
        datetime created_at
        datetime updated_at
    }

    Feedback {
        int id PK
        int submission_id FK
        text feedback
        datetime created_at
        datetime updated_at
    }

    %% Discussion Forum System
    DiscussionCategory {
        int id PK
        string name "unique"
        text description
        string color "hex color"
        boolean is_active
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    DiscussionTopic {
        int id PK
        string title
        text content "Rich text"
        int category_id FK
        boolean is_pinned
        boolean is_locked
        boolean is_active
        int views_count
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    DiscussionReply {
        int id PK
        int topic_id FK
        text content "Rich text"
        int parent_reply_id FK "self-referencing"
        boolean is_active
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    DiscussionView {
        int id PK
        int topic_id FK
        int user_id FK
        datetime viewed_at
    }

    DiscussionLike {
        int id PK
        string content_type "topic or reply"
        int topic_id FK "nullable"
        int reply_id FK "nullable"
        int user_id FK
        datetime created_at
    }

    %% Notification System
    Notification {
        int id PK
        string title
        text message
        int type "1=SystemAlert, 2=Announcement, 3=Deadline, 4=Feedback"
        int created_by_id FK
        datetime created_at
        datetime updated_at
    }

    NotificationUser {
        int id PK
        int notification_id FK
        int user_id FK
        boolean is_read
        datetime created_at
        datetime updated_at
    }

    %% System Configuration
    SystemSettings {
        int id PK
        int last_modified_by_id FK
        int student_per_course
        int mentor_per_course
        int current_term "1-3"
        boolean autorun_week
        int current_week "1-12"
        datetime created_at
        datetime updated_at
    }

    %% Relationships
    User ||--|| Profile : "has"
    
    Department ||--o{ Specialization : "contains"
    Department ||--o{ Profile : "belongs_to"
    Department ||--o{ Course : "offers"
    
    Specialization ||--o{ Profile : "specializes_in"
    Specialization ||--o{ Course : "offers"
    
    SchoolYear ||--o{ Term : "contains"
    SchoolYear ||--o{ Course : "offered_in"
    SchoolYear ||--o{ Profile : "current_year"
    
    Term ||--o{ Profile : "current_term"
    
    User ||--o{ Course : "creates"
    User ||--o{ Module : "creates"
    User ||--o{ Lesson : "creates"
    User ||--o{ Homework : "creates"
    User ||--o{ Milestone : "creates"
    User ||--o{ Submission : "submits"
    User ||--o{ SystemSettings : "modifies"
    
    Course ||--o{ SectionCourse : "has_sections"
    Course ||--o{ CourseMentor : "has_mentors"
    Course ||--o{ CourseStudent : "has_students"
    Course ||--o{ CoursePrerequisites : "requires"
    Course ||--o{ CoursePrerequisites : "prerequisite_for"
    Course ||--o{ Module : "contains"
    Course ||--o{ Milestone : "has"
    
    CourseMentor ||--o{ CourseMentor_mentors : "mentor_assignments"
    CourseMentor_mentors }o--|| User : "mentor"
    
    CourseStudent ||--o{ CourseStudent_students : "student_enrollments"
    CourseStudent_students }o--|| User : "student"
    
    Module ||--o{ Lesson : "contains"
    Module ||--o{ Homework : "has"
    
    Lesson ||--o{ LearningObjective : "has"
    Lesson ||--o{ LessonMedia : "has"
    Lesson ||--o{ LessonLink : "has"
    
    Homework ||--o{ Submission : "receives"
    Milestone ||--o{ Submission : "receives"
    
    Submission ||--o{ Feedback : "receives"
    
    User ||--o{ DiscussionCategory : "creates"
    User ||--o{ DiscussionTopic : "creates"
    User ||--o{ DiscussionReply : "creates"
    User ||--o{ DiscussionView : "views"
    User ||--o{ DiscussionLike : "likes"
    User ||--o{ Notification : "creates"
    User ||--o{ NotificationUser : "receives"
    
    DiscussionCategory ||--o{ DiscussionTopic : "categorizes"
    DiscussionTopic ||--o{ DiscussionReply : "has"
    DiscussionTopic ||--o{ DiscussionView : "tracked_by"
    DiscussionTopic ||--o{ DiscussionLike : "liked_by"
    
    DiscussionReply ||--o{ DiscussionReply : "parent_child"
    DiscussionReply ||--o{ DiscussionLike : "liked_by"
    
    Notification ||--o{ NotificationUser : "sent_to"
    