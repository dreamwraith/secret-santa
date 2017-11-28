Intro
=====
(Requires Python 2.7.x, will not run as is on 3.x)

**secret-santa** can help you manage a list of secret santa participants by
randomly assigning pairings and sending emails. It can avoid pairing 
couples to their significant other, and allows custom email messages to be 
specified.

Note: Forked from Original by underbluewaters on github. Thanks to them for their original work.
Changes from Original:
- Added wishlist functionality, updated config template to include wishlist per member.
- Changed to use platform agnostic datetime formatting (original did not work on windows).
- Changed email message format in YAML to include newlines and wishlist content.
- Included VS2017 Project Files as created under windows environment.


Dependencies
------------

pytz
pyyaml

Usage
-----

Copy config.yml.template to config.yml and enter in the connection details 
for your outgoing mail server. Modify the participants and couples lists and 
the email message if you wish.

    cd secret-santa/
    cp config.yml.template config.yml

Here is the example configuration unchanged:

# Required to connect to your outgoing mail server. Example for using gmail:
# gmail
SMTP_SERVER: smtp.gmail.com
SMTP_PORT: 587
USERNAME: NOOOOOOO@NOOO.COM
PASSWORD: NOOOOOOOOOOOOOO

TIMEZONE: 'US/Pacific'

PARTICIPANTS:
  - Chad <nooo+Chad@gmail.com> [Insert List Objects Here 1]
  - Jen <nooo+Jen@gmail.com> [Insert List Objects Here 2]
  - Bill <nooo+Bill@gmail.com> [Insert List Objects Here 3]
  - Sharon <nooo+Sharon@gmail.com> [Insert List Objects Here 4]


# Warning -- if you mess this up you could get an infinite loop
DONT-PAIR:
  - Chad, Jen    # Chad and Jen are married
  - Chad, Bill   # Chad and Bill are best friends
  - Bill, Sharon

# From address should be the organizer in case participants have any questions
FROM: SecretSantaAdmin <nooo+santaadmin@gmail.com>

# Both SUBJECT and MESSAGE can include variable substitution for the 
# "santa" and "santee"
SUBJECT: Santa {santa}! Your secret santa recipient is {santee}! Read Inside for Details!
MESSAGE: |

  Dear {santa},
  
  This year you are {santee}'s Secret Santa!. Ho Ho Ho!

  As a reminder, this years format is as follows:
  <Replace with your Rules and Instructions>
  
  For the Secret Santa Matching, again your recipient is {santee}!
  The spending target for your secret santa is about $50.00 - below you will find a wishlist/suggested categories of gifts for your recipient.
  <Replace with your Additional Details>

  As a reminder: <Your Reminders>
  
  {santee}'s Wishlist/Gift Ideas
  ===================
  {santee_list}

  
  This message was automagically generated from a computer. 
  
  Nothing could possibly go wrong...
  
  http://github.com/dreamwraith/secret-santa
  

Once configured, call secret-santa:

    python secret_santa.py

Calling secret-santa without arguments will output a test pairing of 
participants.

        Test pairings:

        Chad ---> Bill
        Jen ---> Sharon
        Bill ---> Chad
        Sharon ---> Jen

        To send out emails with new pairings,
        call with the --send argument:

            $ python secret_santa.py --send

To send the emails, call using the `--send` argument

    python secret_santa.py --send