# Teammate Reminders for Jira: user guide

Teammate Reminders is a Jira Cloud app by Design Master Solutions. Set a reminder on any issue for yourself, the assignee, the reporter, the watchers or specific teammates, at an exact date and 24-hour time. This repository holds the public documentation and is the place to report problems.

Install [Teammate Reminders for Jira from the Atlassian Marketplace](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira). Ask a Jira admin to install it on your Jira Cloud site and activate its subscription.

## Set a reminder

1. Open a Jira issue. Below the title, click the **View app actions** icon and choose **Reminders**. The panel stays on the issue.
2. Pick the **Date** and the **Time (24h)**. The list offers 15-minute steps and you can type any minute. Enter times in your browser’s local time zone; changing your Jira profile does not change the browser clock. The shortcuts above the fields fill both in one click: **In 1 hour** and **In 3 hours** (rounded up to the next quarter hour), **Today 17:00** (offered until 16:00), **Tomorrow 09:00** and **Next Monday 09:00**.
3. Under **Remind**, choose Me, Assignee, Reporter or Watchers. Add up to 20 people in **Also remind these people**.
4. Under **How**, choose one:
   - **Private e-mail from Jira**: only the recipients see it. The subject carries the issue key and your note (`Reminder: KEY: note (summary)`), so it reads in the inbox list.
   - **Comment with @mention**: a comment visible to people who can read the issue, mentioning the selected people. Jira controls notification delivery; a posted comment does not prove a bell notification or email arrived. Watchers alone do not resolve to mentions: choose Me, Assignee, Reporter or specific people, or use private email for Watchers.
5. Under **Repeat**, choose Once, Every day, Every weekday (Mon-Fri), Every week or Every month. Every weekday skips Saturday and Sunday in your own time zone, so a Friday reminder comes back on Monday at the same local time. Every month keeps the same day of the month and the same local time; a shorter month uses its last day.
6. Add an optional **Note** and click **Set reminder**.

Recipients are resolved when the reminder is sent, so if the issue changes hands the new assignee gets it. With **Skip if the issue is already done** on, a reminder on a finished issue is not sent, and a repeating reminder stops.

The app checks for due reminders every 5 minutes. Delivery is not an exact-time guarantee: processing and retries can delay it. Check **Apps > My reminders** for the current status.

## See, snooze and cancel your reminders

- **On the issue:** the panel lists every pending reminder on that issue. Only the person who created a reminder can cancel it.
- **Everywhere:** in the Jira sidebar, open **Apps > My reminders** for the reminders you created: **Upcoming** on top, the next one to go out first, and **History** below (sent, cancelled, failed), newest first. **Snooze 1 day** or **1 week** moves an upcoming reminder later, or sends a reminder that already went out once more, one day or one week from now; **Cancel** stops an upcoming one. The status says why a reminder did not go out: **not sent: issue was done** (you chose to skip done issues), **not sent: issue deleted**, **not sent: nobody to notify** (for example, no assignee yet), **retrying (N of 5)** while Jira is not answering, and **failed after 5 tries** after that.

### When the reminder is not yours

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open an issue where a pending reminder already exists.

**Upcoming on this issue** lists every pending reminder on that issue, including one created by someone else. Only the creator can cancel it. If you are not the creator, the reminder stays pending. The app answers:

**Only the person who created this reminder can cancel it.**

Snooze has the same limit. Invoking it on someone else's reminder shows:

**Only the person who created this reminder can snooze it.**

**Apps > My reminders** lists only reminders you created, so Snooze and Cancel there are yours.

Checked on 22 November 2026. `canCancelReminder` allows `C1` to cancel a pending row and refuses `C2` and a sent row. `cancelReminder` answers `C2` with the sentence above. This is not a new click on a customer site.

### What snooze changes

Only the creator can snooze or cancel a reminder. On the issue, a cancel by anyone else is refused. Snooze buttons appear for pending reminders and sent one-off reminders. A future reminder moves 1 or 7 days beyond its existing due time; an overdue or sent reminder moves 1 or 7 days from now. Cancelled reminders must be created again from the issue.

Snoozing a monthly reminder also changes the day used for future repeats. For example, moving a reminder to February 28 makes its next monthly occurrence March 28. Without snoozing, a schedule originally set for the 31st uses the last day of February and returns to March 31.

The sentences Snooze shows when it refuses are in [When Snooze refuses](#when-snooze-refuses).

### A sent one-off keeps Snooze

The Marketplace highlight "Every reminder in one list" shows this screen. On the app, **Snooze 1 day** and **1 week** stay on a reminder that already went out once. The listing caption is "My reminders on 21 Nov 2026. Sent SHOP-6 still has Snooze 1 day and 1 week." The image file on the listing is `2689ffda-3481-4c9c-bcab-751ec1302391`. Atlassian recompressed it, so the bytes differ from the file in this guide.

Measured on 21 November 2026 on the existing Forge development installation. Upcoming was empty: "Nothing scheduled. Snooze a sent reminder or set a new one from an issue." In History, `SHOP-6` was **sent**, shown as **4 Nov 2026, 01:11**, with the note "Refund button is due for review. Please post the status." That row offered **Snooze 1 day** and **1 week**. Cancelled rows on the same page did not. The buttons were not clicked, so the site stored nothing new.

![My reminders on 21 Nov 2026. Upcoming is empty, and sent SHOP-6 still has Snooze 1 day and 1 week.](docs/snooze-sent-one-off.jpg)

What those buttons change comes from the production logic (source `5b668669`), run at `2026-11-22T01:42:54.529Z` and not applied on the site:

- **Snooze 1 day** on a sent one-off makes it pending again, due `2026-11-23T01:42:54.529Z`, retry count 0.
- **Snooze 1 week** on that same sent one-off makes it pending, due `2026-11-29T01:42:54.529Z`.
- **Snooze 1 day** on a pending reminder that is still one hour away moves that due time by 24 hours, to `2026-11-23T02:42:54.529Z`. It does not start the day from the moment you click.

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira). This screen is the development installation, not a new Marketplace install and not a clicked snooze.

### When Snooze refuses

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open **Apps > My reminders**.

**Snooze 1 day** and **1 week** are on a pending reminder, and on a sent reminder that does not repeat. A cancelled row has no Snooze button. The issue panel has no Snooze.

| What you snooze | What you see |
|---|---|
| A cancelled reminder, if Snooze runs after that row was cancelled | **This reminder was cancelled. Set a new one from the issue.** The due time stays. Set a new reminder from the issue. |
| A length other than 1 day or 1 week | **Snooze for 1 day or 1 week.** The two buttons send only those lengths. |
| A new due time more than 366 days after the click | **A reminder can be at most one year ahead.** The due time stays. |

Set reminder uses a different sentence for the same 366-day limit: **The reminder time must be within the next 12 months.** A time exactly 366 days ahead can be saved. Snooze then adds 1 day or 7 days on top of the due time that is already stored. The panel says "one year". The check is 366 days, not a calendar year.

**Example input:** you open My reminders at `2026-11-23T03:40:00.000Z`. A pending reminder is due `2027-11-24T03:40:00.000Z`, which is 366 days after that click. You click **Snooze 1 day**.

**Result:** **A reminder can be at most one year ahead.** The due time stays `2027-11-24T03:40:00.000Z`. **Snooze 1 week** on that same reminder is the same sentence. Nothing new is stored.

A pending reminder due `2027-11-23T03:40:00.000Z` (365 days after the same click) accepts **Snooze 1 day**. The new due time is `2027-11-24T03:40:00.000Z`. **Snooze 1 week** on that 365-day reminder is refused, because 365 plus 7 is 372 days. A pending reminder due `2027-11-17T03:40:00.000Z` (359 days) accepts **Snooze 1 week**, and the new due time is `2027-11-24T03:40:00.000Z`. One millisecond later than that 359-day due time, the week is refused.

A reminder saved at the Set reminder maximum, exactly 366 days ahead, and snoozed at that same moment is the refused example. Wait one day, and **Snooze 1 day** is accepted. The new due time is then 366 days after that later click.

Checked against `snoozed` in production source `d7a2672` on 23 November 2026. This is not a new click on a customer site.

### If a reminder does not arrive

| What you see | What to do |
|---|---|
| **retrying (N of 5)** | The app will try again. Check the status after a later scheduled run before creating a duplicate. |
| **failed after 5 tries** | Automatic retries have stopped. Check that the issue is accessible and contact support with the issue key, due time and status. Once the cause is resolved, set a new reminder from the issue. |
| **not sent: nobody to notify** | Check that the selected role has a person, such as an assignee, or choose a specific teammate. Create a new one-off reminder if needed. |
| **not sent: issue was done** | The skip-done option prevented delivery and stopped any recurrence. Create a new reminder and turn that option off if a completed issue still needs follow-up. |
| **not sent: issue deleted** | Set a reminder on the replacement issue, if one exists. |
| Subscription not active | The issue panel shows the sentence in [When the subscription is not active](#when-the-subscription-is-not-active). **Snooze** on **My reminders** shows the other sentence there. **Cancel** on **My reminders** still runs. |
| Reminders turned off for this project | The form is not on the issue. See [When the project is not on the list](#when-the-project-is-not-on-the-list). |

### When the subscription is not active

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open an issue in an allowed project and click **View app actions > Reminders**.

The issue panel checks the project list first. A project that is off the list shows [When the project is not on the list](#when-the-project-is-not-on-the-list) and does not show this warning.

**Example input:** the Marketplace license is present and not active. The project list is empty, so every project is allowed. You open `SHOP-1`.

**Result:** the panel shows `The Teammate Reminders subscription is not active. Ask a Jira admin to renew it.` There is no date, no **Set reminder**, and no **Cancel** on that issue. Nothing new is stored.

**Apps > My reminders** does not read the license. A pending reminder you already saved still lists **Snooze 1 day**, **1 week**, and **Cancel**.

**Example input:** that page shows a pending reminder due `2026-11-24T12:00:00.000Z`. You click **Snooze 1 day** or **1 week**.

**Result:** the page shows `Your Teammate Reminders subscription is not active. Ask a Jira admin to renew it.` The due time stays `2026-11-24T12:00:00.000Z`. **Set reminder** returns the same sentence if it is invoked while the license is inactive. That check runs before the project list, before the time check, and before the reminder is stored. The issue panel does not offer **Set reminder** while the warning above is on screen.

**Cancel** on **My reminders** still runs. That action does not read the license, so the reminder becomes cancelled and the click does not show the subscription sentence.

A development install with no license object does not show the warning. A missing license is treated as allowed, so **Set reminder** and **Snooze** run.

The scheduled sender in this source does not read the license. This page does not say whether a reminder that is already stored is delivered after the subscription lapses. To stop one, use **Cancel** on **My reminders**.

An inactive subscription does not show the sentences in [When Snooze refuses](#when-snooze-refuses). The license check runs first.

Checked against production source `d7a2672` on 23 November 2026. This is not a new click on a customer site.

### If Set reminder refuses before anything is saved

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open an issue and click **View app actions > Reminders**. These two sentences appear in red on that panel when you click **Set reminder**. The app has not stored a reminder, so **Apps > My reminders** does not gain a row.

| What you see | What to do |
|---|---|
| **You cannot see this issue.** | The app could not confirm that you have Browse projects on that issue. Open an issue you can view, or ask a Jira admin for access. |
| **You cannot comment on this issue. Choose a private email reminder instead.** | You can browse the issue, and **How** is **Comment with @mention**. Comment delivery needs Add comments. Switch **How** to **Private e-mail from Jira**, or ask for permission to comment. Private e-mail is accepted when you can browse the issue. |

**Example input:** you can open the issue, **How** is **Comment with @mention**, and Add comments is off. **Result:** the panel shows the second sentence and nothing is saved. Lack of Add comments does not block **Private e-mail from Jira**. If Browse projects is off, both choices show the first sentence, because that check comes first.

Checked against the production check that runs before the reminder is stored (22 November 2026). This is not a new click on a customer site.

### When Set reminder refuses the time

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open an issue and click **View app actions > Reminders**. These sentences appear in red when you click **Set reminder**. The app has not stored a reminder. It has not asked Jira whether you can browse or comment on the issue. **Apps > My reminders** does not gain a row.

| What you entered | What you see |
|---|---|
| Date or Time is empty, or the browser cannot read that pair | **Pick a date and a time.** |
| A time already past, or less than 1 minute from the click | **The reminder time must be at least 1 minute in the future.** |
| A time more than 366 days after the click | **The reminder time must be within the next 12 months.** |
| **Remind** is empty and **Also remind these people** is empty | **Choose who gets reminded.** |

A time exactly 1 minute ahead is accepted. A time exactly 366 days ahead is accepted. The panel says "12 months". The check is 366 days, not a calendar year.

**Example input:** **Remind** is **Me**, and the time is 30 seconds from the click. **Result:** **The reminder time must be at least 1 minute in the future.** Nothing is saved. Move the time at least one minute ahead and click **Set reminder** again.

If the time is also too soon and nobody is selected, both sentences appear in one line, the time sentence first: `The reminder time must be at least 1 minute in the future. Choose who gets reminded.`

The permission sentences above are checked only after the time and the recipient are accepted.

Checked against `validateReminder` in production source `d7a2672` on 22 November 2026. `createReminder` calls that check before it reads Jira permissions and before it stores the reminder. This is not a new click on a customer site.

Notes can have up to 1,000 characters. The exact red text for a time or a missing recipient is in the section above.

## Worked example: follow up on a supplier every weekday

Use this when an open issue needs a weekday follow-up from whoever owns it at the time. This example reminds a person; it does not create recurring issues.

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), or ask your Jira admin to install it, then try the example on a test issue assigned to you. Review the current Marketplace pricing and subscription terms with your admin before activating it.

**Example input:** an open issue `OPS-42`, titled `Confirm replacement shipment`, with an assignee. The key and people here are illustrative, not a customer record.

1. Open **View app actions > Reminders** on your test issue.
2. Choose a future weekday at **09:00** in your local time. For the fixed example below, use Friday **20 November 2026**, in UTC-3; if that date has passed, pick a future weekday instead.
3. Under **Remind**, select **Assignee** only. Leave **Also remind these people** empty. Choose **Private e-mail from Jira**, then **Every weekday (Mon-Fri)**.
4. Enter `Check supplier confirmation` as the note, keep **Skip if the issue is already done** enabled, and click **Set reminder** once.
5. Open **Apps > My reminders** and find the pending reminder under **Upcoming**. Check the issue key and scheduled time before leaving it running.

**Expected first result:** when the scheduled job processes the due reminder, the app asks Jira to notify the current assignee. If ownership changed after you set it, the new assignee is selected. For the sample issue, the generated subject is:

```text
Reminder: OPS-42: Check supplier confirmation (Confirm replacement shipment)
```

For a successful run on Friday 20 November at 09:00 UTC-3, the next due time is Monday 23 November at 09:00 UTC-3 (12:00 UTC). The repeating reminder stays under **Upcoming** with its next due time; do not expect a separate **History** row for each successful occurrence. The scheduler checks every five minutes, so 09:00 is a due time, not a guaranteed inbox arrival time. A successful Jira request does not prove that an email reached the inbox.

**Finish the test:** click **Cancel** on the upcoming reminder. In a real follow-up, moving the issue to a status in Jira’s Done category before the next run makes the skip-done option stop the recurrence. If it does not arrive, use the recovery table above before creating another reminder.

**Validation scope:** the sample recipient, exact subject, Friday-to-Monday calculation and Done-category detection were checked against the app’s production logic. This is a worked example, not evidence of an email delivered to a real user. Time-zone example assumes a fixed UTC-3 offset.

## Worked example: leave a visible reminder on a test issue

Choose this when the follow-up should remain in the issue's conversation, rather than only in someone's inbox. The note is visible to people who can read that issue; use private email when it should only go to the chosen recipients.

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira). The public listing's **Try it free** opens **Select a site**, followed by **Review and install**. An admin should review the current price and subscription terms before activation. If you see **Request app**, follow the admin-request path instead. An existing installation can go straight to the issue steps below.

1. Open a test issue that you can browse and comment on, in a status outside Jira's Done category.
2. Open **View app actions > Reminders**. Set a time a few minutes in the future, allowing at least one minute before saving. A refused time shows the sentence in [When Set reminder refuses the time](#when-set-reminder-refuses-the-time). A due time is not a guaranteed delivery time; the app checks every five minutes.
3. Keep **Me** as the only recipient and leave **Also remind these people** empty. Choose **Comment with @mention (bell + e-mail)** and **Once**.
4. Enter `QA: confirm the reminder appears in this issue`. Keep **Skip if the issue is already done** enabled. Click **Set reminder** once and verify the saved time under **Upcoming on this issue**.
5. After processing, refresh the issue and open **Activity > Comments**. Look for a comment by Teammate Reminders that mentions your account and contains the exact note. In **Apps > My reminders**, the one-off should move to **History** with **sent**. These are separate checks: do not treat the initial success banner as proof of delivery.

Expected comment, where the account name is an actual Jira mention:

```text
Reminder @your-account: QA: confirm the reminder appears in this issue
```

**If nothing appears:** check My reminders before setting another reminder. With **Watchers** alone, comment mode has no individual recipients and ends with **not sent: nobody to notify**; choose a named person or switch to private email. An unassigned issue also needs another recipient if you selected only Assignee. A Done-category issue is skipped when skip-done is enabled. If **Set reminder** shows "You cannot comment on this issue" or "You cannot see this issue", nothing was saved. See [If Set reminder refuses before anything is saved](#if-set-reminder-refuses-before-anything-is-saved). Retries after a saved reminder are in the table above.

**Finish:** Once does not schedule another occurrence. To stop it before processing, use **Cancel**. Do not click **Snooze** after a successful test unless you want another reminder. The comment is part of the issue history, not a private message.

### Verified QA result, 18 November 2026

On our existing Forge **development** installation, we scheduled the example on test issue `LEG-8` for **23:20 UTC-3**, with **Me**, **Comment with @mention** and **Once**. Jira created comment `10036` at **23:20:25 UTC-3**, authored by **Teammate Reminders for Jira**, with the exact note and a mention of our own account. My reminders changed from **Upcoming (1)** / **pending** to **Upcoming (0)** and a **sent** row in History.

![Actual comment from the QA run, mentioning our account and showing the exact note](docs/comment-reminder-qa.png)

This proves a scheduled comment on the QA installation, not a new Marketplace installation, production subscription activation or email delivery. The screenshot is public so you can inspect the result without access to our test site. [Try the same steps with Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira).

## For Jira admins


**Jira settings > Apps > Teammate Reminders** takes a list of project keys (for example `SHOP, OPS`). Reminders can then be set only in those projects. Empty means every project.

## When the project is not on the list

[Install Teammate Reminders for Jira](https://marketplace.atlassian.com/apps/2781565032/teammate-reminders-for-jira), then open an issue and click **View app actions > Reminders**.

An empty project list allows every project. A saved list allows only those project keys. The issue panel checks that list before it checks the subscription, and before it shows the date, the time, or **Set reminder**.

**Input:** the saved list is `SHOP`. You open `OPS-1`.

**Result:** `Reminders are turned off for this project. A Jira admin can turn them on in Apps, Teammate Reminders.` There is no date field and no **Set reminder**. Nothing is stored, and **Apps > My reminders** does not gain a row. This screen does not show the subscription warning.

`SHOP-12` is allowed when the list contains `SHOP`. The key before the hyphen is what counts, and letter case does not matter.

**To allow the project:** a Jira admin opens **Jira settings > Apps > Teammate Reminders**.

| What the admin saves | What Save says | Which issues show the form |
|---|---|---|
| An empty field | `Reminders are on for every project.` | Every project. |
| `shop, ops` | `Reminders are on for: SHOP, OPS.` | `SHOP-12` and `OPS-9`. `LEG-1` still shows the sentence above. |

A token that is not a project key, such as `1bad`, is left out of the saved list. Someone who is not a Jira admin sees `Only Jira admins can change these settings.` The saved list does not change.

Checked against `isProjectAllowed` and `parseProjectList` in production source `d7a2672` on 22 November 2026. The panel returns the sentence above when `allowed` is false, and that return is before the `licensed` check. This is not a new click on a customer site.

## Data and privacy

The app is Runs on Atlassian: it has no servers of its own and sends no data outside Atlassian. It stores, in Atlassian Forge storage of your site, the issue key, the Atlassian account IDs of the creator and the people chosen, the date, the options and the note of each reminder. It reads issue data only to write the reminder. Once a week it reports the stored account IDs to Atlassian through the Personal data reporting API, and when an account is closed it erases that person's reminders and removes them from other people's. Privacy policy: https://www.designmastersolutions.com/privacy.html

## Pricing

Sold through the Atlassian Marketplace, per Jira user. See the Marketplace listing for the current price.

## Support

- Ticket: https://portal.designmastersolutions.com/submitticket.php
- E-mail: info@designmastersolutions.com

Answers in English or Portuguese.
