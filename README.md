# Get calendar events

This is a very simple Transposit application to demonstrate a few basic functions.

  1. Start by [viewing the app's code](https://console.transposit.com/t/transposit_sample/calendar_events).
  2. Add a key for your Google calendar.
  3. Set the `calendarId` parameter to the email address associated with one of your calendars.
  4. Run the `get_calendar_events` operation. In the **Results** tab, you should see some calendar event data returned in JSON format.

Now, go ahead and fork the app so you can make changes and customize it the way you'd like.

## Constrain the time range

In your newly forked app, let's constrain the time range of calendar data returned, by specifying a `timeMin` per the Google Calendar API. Suppose we want to see events that begin today onward.

Create a new JavaScript operation called `today_date`, and paste in the following code:

```javascript
(params) => {
  return {
    today: new Date()
  };
}
```

Run this JavaScript operation and note the JSON result.

Next, return to the `get_calendar_events` operation, and add an `AND` clause to specify `timeMin`:

```sql
SELECT * FROM google_calendar.get_calendar_events
  WHERE calendarId=@calendarId
  AND timeMin = (select * from this.today_date)
  LIMIT 5
```

Run the operation again, and the calendar entries returned should start no later than today.

## Deploy the operation

  * Click on **Deploy > Endpoints** in the left navigation.
  * Choose the `get_calendar_events` endpoint and set it to **Deployed**.
  * Check the _Require user sign-in_ checkbox.
  * Click the **Save** button at the bottom of the page.

Note that no production key has been provided&mdash;the key you added before is just for development&mdash;so let's set that up next.

## Require user authentication

Click on **Authentication > Production Keys**, and enable the _Require users to authenticate with these data connections_ checkbox for `google_calendar`.

This means when a users tries your app, they'll have to bring their own credentials so they can access their own data.

## Use the operation in a hosted app page

Click on **Code > Page template** to see the template for the hosted app page. Down around line 57 you'll see the operation called is `get_calendar_events`.

Load the live hosted app (find you app's URL from **Deploy > Hosted Page**) and once you've signed in and supplied Google Calendar credentials, you'll see your calendar events there.

Anyone you share your app with can then supply their own credentials and see their own data.