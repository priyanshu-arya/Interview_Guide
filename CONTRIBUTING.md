# 🤝 Contributing to the Interview Guide Repository

Thank you for your interest in contributing to the **Ultimate Technical Interview Preparation Repository**! 

Our goal is to maintain the highest-quality open-source technical interview repository on GitHub, serving candidate software engineers, machine learning engineers, and system architects globally (2026–2027 standard).

---

## 📜 Table of Contents
1. [Code of Conduct & Persona](#-code-of-conduct--persona)
2. [Workflow Rules](#-workflow-rules)
3. [Mandatory Subject Folder Structure](#-mandatory-subject-folder-structure)
4. [File Content Specifications](#-file-content-specifications)
5. [Writing & Quality Standards](#-writing--quality-standards)
6. [How to Submit a Pull Request](#-how-to-submit-a-pull-request)

---

## 🎭 Code of Conduct & Persona

When contributing content to this repository, act as a member of an elite technical team consisting of:
- **Senior Software Engineers & Staff Architects** (15+ years experience)
- **FAANG Hiring Managers & Technical Interviewers**
- **University Professors & Open-Source Maintainers**

### Guiding Principles:
- **Quality Over Quantity**: Comprehensive, production-grade depth is preferred over brief superficial summaries.
- **Zero Filler**: Avoid fluff, generic statements, or repetitive hand-waving. Every sentence must add technical value.
- **Clarity & Empathy**: Write as an experienced mentor teaching ambitious engineers.

---

## ⚡ Workflow Rules

1. **One Subject per Pull Request**: Create or update ONLY one subject folder per PR. Do NOT submit multiple subject folders at once.
2. **Subject Naming**: Folder names must use `CamelCase` or `Snake_Case` with proper capitalization (e.g., `Python/`, `SQL/`, `System_Design/`, `Gen_AI/`).
3. **Strict File Structure**: Every subject directory MUST contain exactly the 7 mandatory files specified below. Missing files will cause PR review rejection.

---

## 📂 Mandatory Subject Folder Structure

Any new or updated subject directory **MUST** contain the following 7 files:

```text
<Subject>/
├── README.md
├── Interview_Guide.md
├── Cheat_Sheet.md
├── Resources.md
├── Top_Questions.md
├── Company_Questions.md
└── Practice_Questions.md
```

---

## 📝 File Content Specifications

### 1. `README.md`
- **Purpose**: Serves as the landing page for the subject.
- **Required Sections**:
  - Subject Overview & Core Concepts
  - Why Top Companies Ask This Subject
  - Prerequisites & Required Knowledge
  - Structured Learning Roadmap & Study Order
  - Time Commitment Guidelines
  - How to Navigate the Subject Folder

### 2. `Interview_Guide.md`
- **Purpose**: Comprehensive technical content divided strictly into 3 levels:
  - **Beginner**: Fundamental concepts, core mechanics, common beginner mistakes, basic interview expectations.
  - **Intermediate**: Frequently tested concepts, coding expectations, real-world edge cases, standard follow-ups.
  - **Advanced**: Production-grade architecture, performance optimization, trade-off analysis, senior/staff interview expectations.

### 3. `Cheat_Sheet.md`
- **Purpose**: High-speed revision sheet designed to be read in under 10 minutes.
- **Required Content**:
  - Core syntax & command cheatsheets
  - High-value Markdown comparison tables (e.g., `Process vs Thread`, `B-Tree vs Hash Index`)
  - Best practices & anti-patterns to avoid
  - Memory tricks, rules of thumb, and quick interview callouts

### 4. `Top_Questions.md`
- **Purpose**: 40–60 carefully curated core interview questions.
- **Structure per Question**:
  - Question & Difficulty rating (`Easy`, `Medium`, `Hard`)
  - Why Interviewers Ask It
  - Detailed Answer & Intuitive Explanation
  - Practical Code/Diagram Example
  - Common Follow-up Questions
  - Common Pitfalls & Candidate Tips

### 5. `Company_Questions.md`
- **Purpose**: Real-world interview patterns derived from hiring trends at leading technology companies (Google, Meta, Amazon, Microsoft, Netflix, Uber, Databricks, etc.).
- **Required Content**:
  - Target Company Category / Tier
  - Problem Statement / Conceptual Theme
  - Expected Depth & Key Solution Indicators
  - Potential Architectural or Algorithmic Follow-ups
  - Candidate Advice & Common Red Flags

### 6. `Practice_Questions.md`
- **Purpose**: Interactive practice material organized by question types.
- **Categorization**:
  - Conceptual Questions
  - Coding Challenges
  - Debugging & Code Repair
  - Output Prediction
  - Scenario-Based / Architectural Questions
  - Tricky Edge-Case Questions
- **Format for Each Entry**: Problem Statement, Difficulty, Hint, Solution Approach, & Common Pitfalls.

### 7. `Resources.md`
- **Purpose**: Hand-picked, high-value learning materials.
- **Categories**:
  - Official Documentation & Specs
  - Recommended Books & Academic Papers
  - Video Tutorials & YouTube Channels
  - Interactive Practice Platforms & Repositories
  - *Note*: Explain briefly *why* each resource is worth using.

---

## 🎨 Writing & Quality Standards

- **Language & Tone**: Clear, precise technical English. Accessible to learners, rigorous enough for staff-level interviewers.
- **Code Snippets**: All code must be syntactically valid, clean, properly formatted, and include commentary explaining key logic.
- **GitHub Markdown**:
  - Use proper header hierarchies (`#`, `##`, `###`).
  - Use GitHub callouts (`> [!NOTE]`, `> [!TIP]`, `> [!IMPORTANT]`, `> [!WARNING]`).
  - Use Markdown tables for side-by-side comparisons.
  - Use Mermaid diagrams (````mermaid ... ````) where flowcharts, sequence diagrams, or architecture schemas add clarity.

---

## 🚀 How to Submit a Pull Request

1. **Fork the Repository**: Click the **Fork** button at the top right of this repository.
2. **Clone your Fork**:
   ```bash
   git clone https://github.com/YOUR_USERNAME/Interview_Guide.git
   cd Interview_Guide
   ```
3. **Create a Feature Branch**:
   ```bash
   git checkout -b feature/add-kubernetes-guide
   # or for updates
   git checkout -b fix/python-asyncio-explanation
   ```
4. **Make Your Changes**:
   - Ensure all 7 files are populated if adding a subject directory.
   - Follow all formatting and quality standards.
5. **Commit Your Changes**:
   ```bash
   git add .
   git commit -m "feat(Kubernetes): add comprehensive 7-file interview guide"
   ```
6. **Push to GitHub**:
   ```bash
   git push origin feature/add-kubernetes-guide
   ```
7. **Open a Pull Request**:
   - Navigate to the original repository on GitHub.
   - Click **New Pull Request**.
   - Provide a clear title and description summarizing your additions/updates.

---

Thank you for helping us build the ultimate technical interview resource! 🌟
