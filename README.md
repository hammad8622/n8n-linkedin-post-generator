# 💬 LinkedIn Post Agent - AI Workflow Automation

This project showcases a fully automated AI-driven content creation and publishing system for LinkedIn using [n8n](https://n8n.io). The **LinkedIn Post Agent** generates professional content via generative AI (Gemini), routes for human approval, and posts directly to LinkedIn—with complete status tracking via Google Sheets.

---

## 🔍 Project Overview

The LinkedIn Post Agent automates LinkedIn content creation and publishing through:

* **AI Generation**: Uses Google Gemini to draft LinkedIn posts.
* **Approval Routing**: Sends posts for human confirmation via Gmail.
* **Multi-Modal Publishing**: Posts with or without images.
* **Status Tracking**: Updates Google Sheets after publishing.

This eliminates manual bottlenecks and ensures fast, consistent publishing workflows.

---

## 🧠 Research & Concept

This agent is an example of a **Task-Oriented Workflow Agent**, defined by:

* **Event-Driven Design**: Triggered manually or on schedule.
* **Single Responsibility**: Create, review, and post LinkedIn content.
* **Autonomous Decision-Making**: AI generates or regenerates posts based on human feedback.

This agent is not focused on customer support or structured data transformation, so those categories don't apply here.

---

## 🔢 Workflow Summary

The workflow operates as follows:

1. Triggered manually or scheduled.
2. Reads pending posts from Google Sheets.
3. Generates post content using Gemini.
4. Sends email to human reviewer.
5. Depending on approval:

   * Posts to LinkedIn with/without image
   * Regenerates content
   * Cancels the process
6. Updates the original Google Sheet row with the status.

---

## 🔹 Node-by-Node Breakdown

### Manual Trigger

* **Type**: `manualTrigger`
* **Purpose**: Starts the process manually.

### Get Data from Sheets

* **Type**: `googleSheets (v4.5)`
* **Purpose**: Retrieves rows with Status = `Pending`.

### Generate Post Content

* **Type**: `langchain.chainLlm (v1.6)`
* **Model**: Google Gemini 1.5-flash
* **Purpose**: Generates LinkedIn post text from description and instructions.

### Data Formatting

* **Type**: `set (v3.4)`
* **Purpose**: Packages generated content with relevant metadata.

### Send Content Confirmation

* **Type**: `gmail (v2.1)`
* **Purpose**: Emails post to reviewer with options (Yes / No / Cancel).

### Content Confirmation Logic

* **Type**: `switch (v3.2)`
* **Purpose**: Routes based on reviewer's response.

### Regenerate Post Content

* **Type**: `langchain.chainLlm (v1.6)`
* **Purpose**: Rewrites the post using reviewer feedback.

### If Image Provided

* **Type**: `if (v2.2)`
* **Purpose**: Decides whether to fetch and attach an image.

### Get Image

* **Type**: `httpRequest (v4.2)`
* **Purpose**: Downloads image using provided URL.

### Post With Image

* **Type**: `linkedIn (v1)`
* **Purpose**: Publishes image and post content to LinkedIn.

### Post Without Image

* **Type**: `linkedIn (v1)`
* **Purpose**: Publishes post content without image.

### Update Google Sheet

* **Type**: `googleSheets (v4.5)`
* **Purpose**: Updates original row with status: `Completed` or `Cancelled`.

---

## 🔄 Data Flow Summary

```text
[Manual Trigger] → [Get Data from Google Sheets]
→ [Generate Content with Gemini]
→ [Email Reviewer for Confirmation]
→ [Handle Response: Post / Regenerate / Cancel]
→ [Post to LinkedIn]
→ [Update Sheet with Status]
```

---

## 🛠️ Tools & Versions

* **n8n Nodes**:

  * `manualTrigger`, `googleSheets (v4.5)`, `langchain.chainLlm (v1.6)`
  * `set (v3.4)`, `gmail (v2.1)`, `switch (v3.2)`, `if (v2.2)`
  * `httpRequest (v4.2)`, `linkedIn (v1)`

* **AI Model**: Google Gemini 1.5-flash

* **External Services**: Gmail, LinkedIn API, Google Sheets

* **Output Format**: Human-readable LinkedIn post text with emoji support

---

## 🔮 Conclusion

The **LinkedIn Post Agent** is a strong demonstration of AI-powered automation for content marketing. It minimizes manual touchpoints, ensures consistent messaging, and keeps stakeholders in the loop.

Scalable and extendable, it can be adapted for other social platforms or more complex approval workflows.

---

## 📝 License

MIT License — free to use, modify, and share.

---

## 🤝 Contributing

Contributions and enhancements welcome. Submit a PR or open an issue.

---

## 🔗 Related Links

* [n8n Documentation](https://docs.n8n.io)
* [Google Gemini](https://deepmind.google/technologies/gemini)
* [LinkedIn Marketing API](https://learn.microsoft.com/en-us/linkedin/marketing/)
* [Google Sheets API](https://developers.google.com/sheets/api)
