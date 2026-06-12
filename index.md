# Introduction #

The Dedham Food Pantry serves more than 1500 Dedham residents annually.
Due to the ever-increasing demand for our services, we implemented a shopping appointment registration system.
It's not bad, but could be much better.

# Background #

The Dedham Food Pantry serves the residents of Dedham, MA.
We serve around 550 households annually, which include around 1500 people.
Those households shop around 7500 times.

We use the Oasis case management system.

Due to increasing demand for our services (currently around 110-120 households on Saturday morning), we had to implement a reservation system.
This allows clients to choose a time to shop, which is more convenient for the clients than "first-come first-served", and also reduces backlogs by smoothing the client arrival rate.

Oasis records shopping events, but doesn't help clients decide when to shop.
It has a scheduling implementation, but it's not usable by clients (only Oasis users) which would waste too much staff time.
We implemented a client-facing reservation system using SignupGenius.

The ideal system would manage *both* cases and appointments but we haven't found one that does.

# History #

As of Spring 2026 we use SignupGenius (SUG), a popular system for managing signups.
SUG works reasonably well, but implements a very basic concept of "signups."
This leads to problems such as:

* The check-in process is awkward because SUG and Oasis aren't integrated
    * Our desk volunteer has to first find the client in SUG (to verify their appointment), then use the client's "blue card" ID to record the shopping event in Oasis
* SUG is not reliable
* The SUG app doesn't integrate with browser password managers
* Maintenance of the available appointments is awkward and time-consuming
* SUG is "open loop": it doesn't track when (or whether) the client honored their appointment

We spend too much time helping our clients manage their SUG accounts.

# Wanna-haves #

* Multilingual (we typically translate everything into English, Spanish, and Haitian Creole)
* Efficient checkin process (integrates with Oasis)
* Tracks whether appointments are honored
* Works with USB barcode scanners (they usually pretend to be keyboards)

# Scale #

Small. 10^3 or 10^4 database rows.

We serve fewer than 200 families per week, usually around 50 on Wednesday and 120 on Saturday.
We don't ask senior citizens to make appointments.

# Use Cases #

## Client Registers For the First Time ##

Clients will register with the system. They'll provide username/password and email.

We need to link the client's account in this system with their account in Oasis.
For the MVP this can be done manually because the info provided by the client might not perfectly match what's in Oasis.
There will need to be an admin UX to add the link.

## Client Makes an Appointment ##

This is the most important UX.
It must be simple and efficient.
It must be responsive on phones and laptops.
Not sure if the MVP needs a tablet mode but that would be nice to have.

It should be available in English, Spanish, and Haitian Creole, with other languages possible.
It should be capable of rendering languages that use multi-byte characters (e.g., Ukranian, Mandarin), even if we don't implement that at first.

The appointment UX should be available only a certain number of hours/days before the shopping event.
Currently we have SUG set to open 48h before shopping.
When the UX is not open for appointments it should explain that, and indicate when it will open.

Clients typically belong to one of two groups:

  * I want the earliest appointment possible
  * I want an appointment at a specific time.

We can save clients time by making the "earliest appointment" workflow as easy as possible.

Clients must be sent an email confirmation of their appointment.

## Client Checks In To Shop ##

The MVP must provide a web page with a chronological list of today's appointments.
Each appointment in the list should have a link to the client's case in Oasis.
Each appointment in the list should have a way to record that the client arrived (to track no-shows).

Post-MVP implementations could support the idea of "queuing" where the desk volunteer can add walkins to the appointment stream.

Integration with our new Pantry TV signage would be cool.

## Volunteer Maintains Appointment Slots ##

The DFP currently has two family shopping events:

  * every Saturday morning (4 hours)
  * the first and third Wednesday in the evening (2 hours)

While we try to keep this schedule, we occasionally need to move a shopping day, for example, if a holiday lands on Saturday.

Each shopping event is divided into 15-minute "slots".
We typically have 6-8 clients per 15-minute slot.
The first slot has fewer clients so we can ramp up at the beginning of a shopping event.

We have shopping events for senior citizens but we don't ask them to make appointments.
Senior shopping is first-come first-served.

An ongoing SUG pain point has been that a volunteer must explicitly set up each 15-minute slot in each event.
The SUG UX makes it easy to make a mistake that takes a lot of effort to correct (e.g., create a batch of things that can only be corrected one-by-one).

Since we have a fixed "rhythm" we shouldn't need so much manual configuration.

## Reporting ##

  * Printable report of today's appointments (in case the internet goes down during shopping)
  * "No-show" report
