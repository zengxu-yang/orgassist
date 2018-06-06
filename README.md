What is it?
===========
"orgassist" is a bot - an assistant who handles your appointments, tasks and
note-taking when you're away from your computer. It can integrate multiple
sources of notifications and use multiple different communication interfaces -
by default XMPP.

Currently working functions include:
- Bidirectional communication over XMPP.
- Notifying in advance about planned appointments.
- Generating and sending an agenda for the current day.
- Reading events from org-mode tree (schedules, deadlines, appointments)
- Reading events from Exchange calendar.
- Taking notes and storing in an org-mode inbox file.

It's architected to be easily expandable, currently planned features:
- other bot-interfaces: irc interface, email interface, web interface,
  android push-notification interface,
- caldav integration,
- rescheduling tasks and snoozing notifications using remote commands,
- intelligent capturing which handles tags and dates,
- changing task state,
- incremental search,
- etc.


But why?
===========

* Do you love your org-mode, but still struggle to get the agenda or
  notifications on your two mobile devices?
* You have two org-mode trees - one for work, one for private planning?
* And appointments in Outlook or Google Calendar?
* And sticky notes or notepad to gather notes on the run?
* Or maybe a mobile app to gather notes (orgzly?)
* Taking notes on the run requires you to later integrate them?
* You treat your org-mode as private notes and dislike keeping them decrypted
  everywhere, but at the same time would like to use it remotely?

I had most of those problems and decided this would be an elegant way to solve
all of them without dropping org-mode or using cloud-sync solutions.


Plugins
===========
OrgAssist is split into plugins with a well-defined API.

Calendar
-----------
It's a "Core" plugin since its existence is a depedency of other plugins.

It manages a list of dated events with a state (TODO, DONE, etc.) in a common
format. For this events, generates notifications and agenda views.

Org
-----------
Reads org files and feeds events into the calendar. Handles command to take
notes.

Planned: changing state of tasks, rescheduling, smarter capture.

Exchange
-----------
Fills calendar with events from your company's Exchange. Detects those set by
you, and with your required and optional attendance.

Planned: detecting new events.

Planned: Shell
-----------
Execute a configured shell command on request. Enable/disable alarms, control
music, etc.


Setup
===========
Tested with Python 3.5 and 3.6.

1. pip3 install orgassist
2. assist.py --generate-config
3. emacs/vim ~/.org/orgassist.yml - configure XMPP accounts, boss JID, org-mode
   directory, etc. See comments in the config file for ideas.
3. Run bot: $ assist.py --config ~/.org/orgassist.yml

Developing own plugins
==========
See `example_plugin.py` for an example and showcase of the API. You can develop
plugins using the PyPI version of orgassist by specifying config parameters
`plugins_path` and `plugins`.

Architecture
-------
Single orgassist instance can have multiple interfaces (xmpp, irc) with multiple
assistants connected to them. Each assistant handles a single "boss" -
identified by JID or irc nick/realname. Each assistant can have different
plugins enabled, with different configuration and state.

                                                 /- Calendar Plugin
    Interfaces  --> | Assistant 1 (Boss JID 1) -+
    (xmpp, irc)     | state, config              \- Org plugin
                    |
                    |                            /- Calendar plugin
                    | Assistant 2 (JID 2) ------+
                    |                            \- Org plugin, OWA Plugin
                    | Assistant 3 ---> etc.


License
=======
License: MIT License.
Author: Tomasz Fortuna, 2019.
Contact: bla@thera.be

Orgassist includes an external MIT-licensed module "orgnode" by Albin Stjerna,
Takafumi Arakaki, and Charles Cave (https://github.com/albins/orgnode.git).
Edited by myself to cleanup API and fix some problems.
