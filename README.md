# Teammate Reminders for Jira: user guide

Teammate Reminders is a Jira Cloud app by Design Master Solutions. Set a reminder on any issue for yourself, the assignee, the reporter, the watchers or specific teammates, at an exact date and 24-hour time. This repository holds the public documentation and is the place to report problems.

## Set a reminder

1. Open a Jira issue. Below the title, click the **View app actions** icon and choose **Reminders**. The panel stays on the issue.
2. Pick the **Date** and the **Time (24h)**. The list offers 15-minute steps and you can type any minute. Times use your Jira/browser time zone.
3. Under **Remind**, choose Me, Assignee, Reporter or Watchers. Add up to 20 people in **Also remind these people**.
4. Under **How**, choose one:
   - **Private e-mail from Jira**: only the recipients see it, and the subject carries the issue key.
   - **Comment with @mention**: a comment on the issue that mentions the recipients, so they get the Jira bell and the e-mail.
5. Under **Repeat**, choose Once, Every day or Every week.
6. Add an optional **Note** and click **Set reminder**.

Recipients are resolved when the reminder is sent, so if the issue changes hands the new assignee gets it. With **Skip if the issue is already done** on, a reminder on a finished issue is not sent, and a repeating reminder stops.

Reminders go out within 5 minutes of the chosen time.

## See and cancel your reminders

- **On the issue:** the panel lists what is upcoming there, with a **Cancel** button.
- **Everywhere:** in the Jira sidebar, open **Apps > My reminders** for the reminders you created, with their status.

## For Jira admins

**Jira settings > Apps > Teammate Reminders** takes a list of project keys (for example `SHOP, OPS`). Reminders can then be set only in those projects. Empty means every project.

## Data and privacy

The app is Runs on Atlassian: it has no servers of its own and sends no data outside Atlassian. It stores, in Atlassian Forge storage of your site, the issue key, the Atlassian account IDs of the creator and the people chosen, the date, the options and the note of each reminder. It reads issue data only to write the reminder. Privacy policy: https://www.designmastersolutions.com/privacy.html

## Pricing

Sold through the Atlassian Marketplace, per Jira user. See the Marketplace listing for the current price.

## Support

- Ticket: https://portal.designmastersolutions.com/submitticket.php
- E-mail: info@designmastersolutions.com

Answers in English or Portuguese.
