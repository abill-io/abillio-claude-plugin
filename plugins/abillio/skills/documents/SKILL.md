---
name: documents
description: Find Abillio documents, review signature status, manage document templates or automation rules, and prepare a document from a template.
---

# Abillio documents

Read [shared workflow rules](../../references/workflows.md) and
[capability limits](../../references/capability-status.md).

Use `list_generated_documents`, `get_generated_document_detail` and
`list_document_acceptances` to identify the document, participants and actual
signature state. A generated document is not a signed agreement; a signature
request is not acceptance. Do not expose fields or participant evidence the
connection does not return.

Resolve a template with `list_document_templates`; inspect
`list_document_placeholders` before adding tokens. Previews and file content must
be opened directly in Abillio. Use `create_generated_document` only when a saved
document is requested, with the intended template and invoice/context. Read back
the returned document identifier and state before claiming completion. Content
inspection happens in Abillio; this plugin does not return the rendered document.

For template edits, distinguish account-owned templates from protected Abillio
built-ins. Use the create/patch tool for the explicit request. Deleting an unused
template differs from archiving one used by documents or rules; a 409 in-use result
must not be bypassed.

Automation rules can generate documents or require contracts for existing/future
engagements. Establish purpose, trigger, affected people and activation intent.
Prepare a rule disabled unless the user already authorized activation; preview
its impact with the available impact/freelancer tools before enabling it. Do not
invent signatories, saved signatures, grace periods or legal text.

Signing, rejecting personal agreements and accepting mandates require the human
workflow in Abillio. Do not use document generation, a pasted signature image or
another credential to imitate signing. PDF/ZIP downloads and raw file content are not exposed by this public plugin.
Open files in Abillio; do not fetch authenticated URLs using another tool.
