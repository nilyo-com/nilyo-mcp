---
name: nilyo-recruiting
description: "Use when the user recruits through THEIR OWN LinkedIn account via Nilyo: 'applicants for my job posting', 'show this candidate's resume', 'source senior React developers in Lyon', 'my Recruiter projects', 'contact this candidate', 'search jobs', talent pipelines and hiring follow-ups. Covers LinkedIn job postings (Classic), Recruiter and Sales Navigator search. Do not use for sales prospecting (outreach skill) or content (content skill)."
---
# Recruiting with Nilyo

Nilyo gives ChatGPT the user's own LinkedIn account, including Recruiter and job postings when the account has them. Candidate data is personal data: return only what the user asked for.

## Applicants on the user's job postings (LinkedIn Classic)
1. `linkedin_list_job_postings` → `job.id` (never a job title as id).
2. `linkedin_classic_list_job_applicants(job_id)` → `applicant.id`; `linkedin_classic_get_job_applicant` for the profile; `linkedin_classic_get_applicant_resume` for the CV (summarize, do not paste it whole unless asked).
3. Show the applicants with the facts the user asks for; the user decides who to contact. Nilyo does not score, rank or screen candidates. For each candidate the user picks, propose a message and send it after approval with `linkedin_send_message` / `linkedin_start_conversation` (InMail from the right inbox when needed).

## Recruiter
- Contracts: `linkedin_list_contracts` → `linkedin_select_contract` when several.
- Sourcing: `linkedin_recruiter_search_people(keywords, title, location, skills…)`; projects and pipelines: `linkedin_recruiter_list_applicants(project_id)`, `linkedin_recruiter_get_applicant`, `linkedin_recruiter_get_applicant_resume`.
- Without Recruiter: `linkedin_search_people` (Classic) or `linkedin_sales_navigator_search_people`; the result explains which products the account has.

## Jobs
`linkedin_search_jobs(keywords, location)` when the user looks for openings (their own market research or a candidate's).

## Outreach to candidates
Resolve the person (`profile.id`), read any previous conversation (`linkedin_read_conversation`), draft a short personal message, wait for "send". Invitations with a note ≤ 300 characters via `linkedin_send_invitation`.

## Rules
- Rate limits: ~100 LinkedIn actions per day including profile views and searches; one precise search beats ten broad ones; never bulk-message candidates.
- Opt-outs: When someone asks not to be contacted ("stop", "not interested", "remove me"), stop: do not message, invite or follow up with them again, and tell the user.
- IDs: job_id, project_id and applicant.id come from the listing tools; a name or title is never an id.
- Keep candidate summaries factual (role, company, years, skills, location); no speculation on protected characteristics.
