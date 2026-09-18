---
name: nilyo-linkedin-content
description: Use when the user wants to publish, comment, react to or monitor LinkedIn content from THEIR OWN account through Nilyo: 'write a LinkedIn post', 'comment on this post', 'who reacted to my post', 'reply to the comments', 'what did X publish recently', 'like this', 'follow this company', engagement routines, content ideas grounded in their network. Do not use for direct messages or invitations (outreach skill) or for generic social-media advice without an account action.
---
# LinkedIn content & engagement with Nilyo

Nilyo gives ChatGPT the user's own LinkedIn account. Every publication is public and permanent: show the text, get an explicit approval, then act.

## Publish
1. Draft the post (hook, body, call to action, hashtags only if the user likes them), show it.
2. `linkedin_create_post(text, …)` after approval; a company page post needs the page from `linkedin_list_managed_company_pages`.
3. Later: `linkedin_resolve_my_post` → `post.id` for edits (`social_update_post`), deletion (`social_delete_post`, only on explicit request), reactions (`linkedin_list_post_reactions`) and comments (`linkedin_list_post_comments`).

## Engage
- Find content: `linkedin_list_user_posts(profile.id)` for a person or company, `linkedin_search_posts(keywords)` for a topic. Resolve people first (URL → `linkedin_get_profile_from_url`, name → `linkedin_search_people`).
- Read before writing: `linkedin_get_post`, then `linkedin_list_post_comments` / `linkedin_list_comment_replies` so a reply fits the thread.
- Act only after approval: `linkedin_comment_on_post`, `linkedin_reply_to_comment`, `linkedin_react_to_post` (reaction `linkedin_like`, `linkedin_celebrate`, `linkedin_support`, `linkedin_love`, `linkedin_insightful`, `linkedin_funny`), `social_update_comment` / `social_delete_comment` for the user's own comments.
- Network: `linkedin_list_followers`, `linkedin_list_following`; follow/unfollow only when asked.

## Routines
"Engage with my prospects every morning": list the people, fetch their latest post each, propose one comment per post, wait for approval, then comment. Keep it to a handful per day.

## Rules
- LinkedIn budget: ~100 actions per day including reads and searches, calls serialized. Never mass-comment or mass-like; propose a small reviewed batch.
- Never invent quotes or facts about a person's post; quote the actual content returned.
- `PROFILE_NOT_ACCESSIBLE` = private profile, do not retry. Results carry `next_tools` and corrective errors: follow them.
