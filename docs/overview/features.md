# Features

This page summarizes what the app provides today. Use it to decide whether a self-hosted deployment meets your repair cafe's needs.

## User authentication

- Secure login for **admin** and **volunteer** accounts
- JWT-based sessions
- Accounts are created manually in MongoDB (no public registration)

<img src="images/login-email.png" width="200px">
<img src="images/login-password.png" width="200px">

## Repair events

- Create and manage repair cafe events (dates, locations)
- View and edit repairs within an event
- Web-app uses local timezone/locale

<img src="images/events-list.png" width="200px">
<img src="images/events-add-edit.png" width="200px">

## Repair logging

- Log items brought in for repair (category, description, owner info)
- Track repair status (e.g. in queue, in progress, completed)
- Assign repairers (volunteers) to repairs
- Flag follow-up repairs
- Record weight and cost estimates (units configurable via env vars)

<img src="images/repairs-list.png" width="200px">
<img src="images/repair-add-01.png" width="200px">
<img src="images/repair-add-02.png" width="200px">
<img src="images/repair-add-03.png" width="200px">

## Volunteer tracking

- Add and edit volunteers for each event
- Re-use previous volunteers from past events when adding new ones
- Track volunteer roles and contact info

<img src="images/volunteers-list.png" width="200px">
<img src="images/volunteer-add.png" width="200px">

## Terms of service / waivers

- Display and record acceptance of waivers for item owners and volunteers

<img src="images/terms.png" width="200px">

## Stats Dashboard

- Display statistics for past repair events

<img src="images/dashboard-01.png" width="200px">
<img src="images/dashboard-02.png" width="200px">
<img src="images/dashboard-03.png" width="200px">
<img src="images/dashboard-04.png" width="200px">

## About Page

- A simple about page from field in the database in HTML format

<img src="images/menu.png" width="200px">
<img src="images/about.png" width="200px">

## Web-first

The primary deployment target is the **web build** (`npm run build:web`), suitable for laptops, phones, and tablets at repair events. Native iOS/Android builds are possible via Expo but are not the focus of this documentation.

## Related pages

- [Local development](../getting-started/local-development.md)
- [First admin user](../operations/first-admin-user.md)
