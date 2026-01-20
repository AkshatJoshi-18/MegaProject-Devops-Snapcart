# 📔 DevOps Engineering Journal

**Author:** Akshat Joshi
**Project:** Snapcart Infrastructure (Terraform + AWS + GitHub Actions)
**Goal:** Documenting the journey of building a production-grade infrastructure from scratch.

---

## 📅 Day 1: Secure CI Pipeline & OIDC Authentication
**Date:** January 20, 2026

### 🚀 Summary
Today, I successfully built a **Continuous Integration (CI) pipeline** for my Terraform infrastructure. I moved away from using risky Access Keys and implemented **OIDC (OpenID Connect)** for secure, password-less authentication between GitHub and AWS. I also adopted professional workflows like **Trunk-Based Development** and **Conventional Commits**.

### 🧠 Key Technical Learnings

#### 1. AWS IAM & Security Engineering
* **OIDC Nuances:** I learned that OIDC Trust Policies are extremely strict.
    * **Case Sensitivity Matters:** `AkshatJoshi` is not the same as `Akshatjoshi`. The GitHub username in the policy must match the payload exactly.
    * **No URLs in Condition:** The `sub` (subject) field expects `repo:Owner/Repo:*`, not the full HTTPS URL.
* **Segregation of Duties:** In a production environment, roles should be split:
    * **CI Role (The Planner):** Read-only access, trusted to run on Feature Branches.
    * **CD Role (The Builder):** Admin access, locked strictly to the `main` branch.
* **Privilege Escalation Prevention:** I implemented a sophisticated IAM policy that allows Terraform to create roles (`iam:CreateRole`) **only** if they start with a specific prefix (`snapcart-*`). This prevents the pipeline from creating unauthorized Admin users.

#### 2. Advanced Git & Version Control
* **Recovering from "Large File" Errors:**
    * I accidentally tried to push 700MB+ of Terraform provider plugins, hitting GitHub's 100MB limit.
    * **The Fix:** Used `git reset HEAD~1` (undo commit) and `git rm -r --cached .` (clear cache) to fix the repo without deleting work.
* **Repo Hygiene:** Configured `.gitignore` to strictly exclude `.terraform` folders (bloat) and `*.tfstate` files (security risk).
* **Trunk-Based Development:** Adopted the **GitHub Flow** method:
    * Create Feature Branch (`feature/setup-ci`) → Open PR → Merge to `main` → **Delete Branch**.
    * *Rule:* Deleting the branch after merge is critical to keep the repo clean.
* **Conventional Commits:** Adopted industry-standard commit messages:
    * `feat:` for new infrastructure.
    * `fix:` for bug repairs.
    * `chore:` for maintenance (like updating `.gitignore` or pipeline YAMLs).

#### 3. Terraform Best Practices
* **Version Locking:** The CI pipeline's Terraform version **must** match the local laptop version (`1.14.3`) exactly. Mismatches lead to state file corruption.
* **Automated Quality:** Integrated `terraform fmt -recursive` into the pipeline to enforce clean code standards automatically.
* **The "State" Problem:** Realized I cannot run `terraform apply` in GitHub Actions yet because runners are ephemeral. I need a **Remote Backend** (S3 + DynamoDB) to give the pipeline a permanent memory.

#### 4. CI/CD (GitHub Actions)
* **Permissions:** By default, GitHub Actions cannot create Pull Requests. I had to manually enable "Allow GitHub Actions to create and approve pull requests" in the repository settings.
* **Fail-Fast Logic:** My pipeline runs cheap/fast checks (`fmt`, `validate`, `tfsec`) first. It only runs the expensive `terraform plan` if the code passes basic quality checks.

### ⚠️ Challenges & Debugging
| Error | Cause | Solution |
| :--- | :--- | :--- |
| `Could not assume role with OIDC` | Typo in Trust Policy (capital 'J') & invalid URL format. | Corrected JSON to match exact OIDC token format. |
| `Terraform exited with code 3` | Code formatting issues. | Ran `terraform fmt -recursive` locally. |
| `GitHub Actions not permitted to create PR` | Security setting disabled by default. | Updated Workflow permissions in GitHub Settings. |

### 🔮 Next Steps
1.  **Remote Backend:** Set up S3 (storage) and DynamoDB (locking).
2.  **CD Pipeline:** Build the workflow to provision infrastructure on merge to `main`.

---