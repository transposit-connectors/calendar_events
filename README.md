# Get calendar events

This is a very simple Transposit application to demonstrate a few basic functions.

  1. Start by [viewing the app's code](https://console.transposit.com/t/transposit-sample/calendar_events?readme=true).
  2. Add a key for your Google calendar.
  3. Set the `@calendarId` parameter to the email address associated with one of your calendars.
  4. Run the `get_calendar_events` operation. In the **Results** tab, you should see some calendar event data returned in JSON format.

Now, go ahead and fork the app so you can make changes and customize it the way you'd like.

## Constrain the time range

In your newly forked app, let's see how you can constrain the time range of calendar data returned, by specifying a `timeMin` per the Google Calendar API. Suppose we want to see events that begin today onward.

Take a look at the JavaScript operation `today_date`, with the following code:

```javascript
(params) => {
  return {
    today: new Date()
  };
}
```

Run this JavaScript operation and observe the JSON result.

Next, return to the `get_calendar_events` operation, and add an `AND` clause to specify `timeMin`:

```sql
SELECT * FROM google_calendar.get_calendar_events
  WHERE calendarId=@calendarId
  AND timeMin = (select today from this.today_date)
  LIMIT 5
```

Run the operation, and the calendar entries now returned should start no later than today.

## Deploy the operation

  * Click on **Deploy > Endpoints** in the left navigation.
  * Choose the `get_calendar_events` endpoint and set it to **Deployed**.
  * Check the _Require user sign-in_ checkbox.
  * Click the **Save** button at the bottom of the page.

## Require user authentication

Next, let's set it up so when users try your app, they can bring their own credentials and access their own data.

Click on **Authentication > Production Keys**, and enable the _Require users to authenticate with these data connections_ checkbox for `google_calendar`.

## Use the operation in a hosted app page

Click on **Code > Page template** to see the template for the hosted app page. Down around line 57 you'll see the operation called is `get_calendar_events`.

Load the live hosted app (find your app's URL at **Deploy > Hosted Page**) and once you've signed in and supplied Google Calendar credentials, you'll see your calendar events there.

Anyone you share your app with can then supply their own credentials and see their own data.