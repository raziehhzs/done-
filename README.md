
import os
from github import Github, GithubException +

def get_github_client:
    token  os.getenv("GITHUB_TOKEN")
    if not token:
        raise RuntimeError("توکن گیت‌هاب پیدا نشد. لطفاً GITHUB_TOKEN را تنظیم کن.")
    return Github(token)

def list_user_repos(gh):
    user = gh.get_user()
    print(f"Repositories for {user.login}:")
    for repo in user.get_repos():
        print(f"- {repo.full_name}  (private={repo.private})")

def create_repo(gh, name, private=True, description=""):
    user = gh.get_user()
    try:
        repo = user.create_repo(name=name, private=private, description=description)
        print(f"Repository created: {repo.full_name}")
        return repo
    except GithubException as e:
        print("خطا در ایجاد ریپو:", e.data)
        raise

def create_issue(gh, repo_full_name, title, body=""):
    try:
        repo = gh.get_repo(repo_full_name)
        issue = repo.create_issue(title=title, body=body)
        print(f"Issue created: #{issue.number} {issue.title}")
        return issue
    except GithubException as e:
        print("خطا در ساخت issue:", e.data)
        raise

def update_profile_bio(gh, new_bio):
    user = gh.get_user()
    try:
        user.edit(bio=new_bio)
        print(f"Bio updated to: {new_bio}")
    except GithubException as e:
        print("خطا در به‌روزرسانی پروفایل:", e.data)
        raise

if __name__ == "__main__":
    gh = get_github_client()

    # مثال‌ها:
    list_user_repos(gh)

    # ساخت یک ریپو جدید:
    # create_repo(gh, "test-repo-from-script", private=True, description="Created by script")

    # ساخت یک issue:
    # create_issue(gh, "yourusername/test-repo-from-script", "Test issue", "این یک issue تست است.")

    # بروزرسانی بیو:
    # update_profile_bio(gh, "سلام! این بیو از طریق API آپدیت شده.")

