# 🚀 **Power Pages Partial Deployment: A Smarter Approach to Portal Deployment**  

## **🔍 Overview**  

Deploying Power Pages portals can be **time-consuming**—especially when dealing with large configurations where only a few components change. A **full portal deployment** is **often unnecessary** and adds complexity.  

This is where **Partial Deployment** comes in! With this approach, teams can:  

✅ Deploy **only the changed components**, significantly reducing deployment time.  
✅ Ensure **fast and efficient** updates without impacting the rest of the portal.  
✅ Still perform **full deployments** when necessary (e.g., major changes, cleanup).  

---

## **📌 Use Cases**  

1️⃣ **Partial Deployment** → Deploy only modified or new components (e.g., web templates, content snippets, deployment profiles) **within a specific timeframe**.  
2️⃣ **Full Deployment** → If components have been **removed** or a complete rebuild is needed, deploy the entire portal structure.  

---

## **⚙️ How It Works**  

🔹 **Mode 1: Partial Deployment** → The pipeline detects modified files **within X days** and only packages those.  
🔹 **Mode 2: Full Deployment** → If `numberOfDays = ALL` or files have been **deleted**, a full deployment is triggered.  

🎯 **Key Mechanism:** A Git-based check identifies changes and deleted files:  
```powershell
$deletedFiles = git log --since="$days days ago" --diff-filter=D --pretty=format: --name-only -- "IRIS/paportal/iris/"
if ($deletedFiles) { 
    echo "##vso[task.setvariable variable=FULL_BUILD_REQUIRED;]true"
} else {
    echo "##vso[task.setvariable variable=FULL_BUILD_REQUIRED;]false"
}
```

🔹 **Ensures key files are included** (`website.yml`, deployment profiles).  
🔹 **Filters related YAML files** to maintain dependencies.  

📌 **Comparison with Existing Methods**  

| **Method**                     | **Pros** | **Cons** |
|---------------------------------|----------|----------|
| ✅ **Partial Deployment (Git-based)** | Efficient, automatic, flexible | Needs Git history tracking |
| ❌ **pac CLI (Power Pages exclude entities)** | Works with Environment | Cannot build from repository |
| ❌ **Manifest Files** | Defines dependencies explicitly | Requires manual updates, bloated file size |

⚠️ **Why Not Manifest Files?**  
- Needs to be **manually maintained** per environment.  
- Becomes **heavy and difficult** to track over time.  
- If outdated, can **break deployments** due to missing dependencies.  

---

## **🔄 Step-by-Step Pipeline Process**  

1️⃣ **Git Checkout** → Fetches portal files from the repository.  
2️⃣ **Change Detection** → Identifies modified files **within the last X days**.  
3️⃣ **Include Dependencies** → Ensures mandatory deployment profiles & YAML files.  
4️⃣ **Full Deployment Check** → If `ALL` is set or files were deleted, deploy everything.  
5️⃣ **Package & Publish** → Uploads the selected artifacts for deployment.  

---

## **🚀 Why This Approach?**  

✅ **Fast & Efficient** → Reduces deployment time by processing only necessary files.  
✅ **Flexible** → Supports both **incremental updates** and **full rebuilds**.  
✅ **Automated & Reliable** → Avoids unnecessary manual intervention.  

