# Skill Card

## Description

Yandex Tracker reads and manages issues, queues, comments, attachments, links, worklogs, planning data, and bulk changes for authorized Yandex Tracker users.

This unreleased feature-branch version is intended for review and controlled testing. Production use should follow organization-specific security, permission, evaluation, and release review.

## Owner

Artem Kovalchuk ([Kandler3](https://github.com/Kandler3)) is the repository owner and accountable maintainer.

## License/Terms of Use

The skill is licensed under the [MIT License](LICENSE), SPDX identifier `MIT`.

Use of Yandex Tracker and `yandex_tracker_client` is also subject to their respective terms and organizational policies.

## Use Case

The skill is intended for people and teams that already have authorized access to Yandex Tracker and want an agent to:

- retrieve and summarize issue data;
- create, update, and transition issues;
- work with comments, attachments, links, and worklogs;
- inspect queues, users, boards, and sprints;
- perform explicitly scoped bulk changes.

It is not an authorization mechanism and must not be used to bypass Tracker permissions or organizational approval requirements.

## Deployment Geography for Use

The repository owner has not specified a geographic deployment scope. Use the skill only where Yandex Tracker is available and its use is permitted by applicable law, Yandex terms, and the deploying organization's data-handling policies.

### Requirements / Dependencies

- Python 3 and the `yandex_tracker_client` package.
- Network access to the Yandex Tracker API.
- Access to the filesystem-based skill directory.
- A Yandex account authorized for the target organization and queues.
- `TRACKER_TOKEN` plus exactly one of `TRACKER_ORG_ID` or `TRACKER_CLOUD_ORG_ID`.

Requires API Key or External Credential: **Yes**

Credential Type(s): **OAuth token and organization identifier; temporary Yandex Cloud IAM token may be used when configured explicitly**

Keep credentials in the runtime's secret or environment mechanism. Do not include secrets in prompts, scripts, logs, output, or repository files. Use least-privilege credentials and rotate or revoke them according to organization policy.

## Known Risks and Mitigations

### Unauthorized or overly broad changes

**Risk:** The skill can create, update, transition, delete, or bulk-change Tracker data, including many issues at once.

**Mitigation:** Require clear user authorization and scope for mutations. Inspect the current state, preview broad issue sets, verify queue-specific values, and re-read affected issues after high-impact changes.

### Credential disclosure

**Risk:** OAuth or IAM credentials could be exposed through prompts, generated scripts, logs, or unredacted API output.

**Mitigation:** Read credentials only from runtime-managed secrets or environment variables. Never print or persist credentials, authorization headers, or sensitive raw payloads.

### Incorrect queue-specific values

**Risk:** Custom field keys, transition IDs, resolutions, users, components, and sprint IDs differ between Tracker configurations; guessed values can fail or modify the wrong data.

**Mitigation:** Discover valid values from the target queue, issue, or API catalog before mutation. Do not infer identifiers from display names when ambiguity exists.

### Partial or asynchronous bulk failure

**Risk:** A bulk operation may remain in progress, fail partially, or produce a state different from the requested change.

**Mitigation:** Wait for bulk completion, require a successful status, surface `statusText` on failure, and verify affected issues before reporting success.

### Unintended notifications or data exposure

**Risk:** Comments, mentions, attachments, user records, and worklogs may notify people or expose personal and project information.

**Mitigation:** Confirm recipients, issue keys, attachment paths, and requested fields. Return only information needed for the task and follow the organization's access and retention rules.

### Dependency and API changes

**Risk:** Unpinned client releases or Yandex Tracker API changes may alter behavior after the skill is reviewed.

**Mitigation:** Review dependency changes, pin versions when reproducibility is required, and repeat validation and controlled tests before release.

## References

- [Source repository](https://github.com/Kandler3/yandex-tracker-openclaw-skill)
- [`yandex_tracker_client` package](https://pypi.org/project/yandex-tracker-client/)
- [Anthropic skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [NVIDIA skill card guidance](https://docs.nvidia.com/skills/skill-cards)

## Skill Output

Output type(s): **Yandex Tracker API calls, Tracker-side mutations, and user-facing reports**

Output format: **Concise Markdown or plain text by default; structured JSON or tables when requested or useful**

Output parameters: **Issue keys, requested fields, counts, grouped summaries, created resource identifiers, transition results, or bulk-change status, depending on the request**

Other properties: **Mutating requests can change shared Tracker data and trigger notifications. The skill should report affected issue keys and confirmed outcomes. It does not require a persistent local output file.**

## Skill Version

**Version `1.1.0` — unreleased, prepared on `feat/skill-structure-and-docs`.**

## Release Evidence

- Source branch: `feat/skill-structure-and-docs`
- Skill structure validation: `Skill is valid!` from `quick_validate.py`
- SkillEvaluator / SkillSpector report: not yet generated
- Evaluation dataset and benchmark report: not yet generated
- Release tag and OMS signature: not yet created

## Ethical Considerations

Use human review for mutations that affect shared workflows, many issues, or other users. Respect privacy and workplace governance when accessing user profiles, emails, comments, attachments, and worklogs. Do not use activity data as a sole basis for employee evaluation, disciplinary decisions, or other high-impact judgments. Do not use the skill to evade authorization, conceal changes, or extract data beyond the user's legitimate access and stated task.
