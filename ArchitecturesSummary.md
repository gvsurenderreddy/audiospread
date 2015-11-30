# Summary of the AudioSpread servers architecture #

## AudioSpread Drupal server ##

  * the **Drupal CMS** over **Linux+Apache+MySQL+PHP** (LAMP) server server. This server is a base for the following elements:

  * a **local file storage**:
    * to store temporarily audio files sent through Drupal
    * to store recordings from Asterisk server until publishing or user deletion.
    * files stay here until they get uploaded to the Asterisk server, where they will stay definitely.

  * a **MySQL database used by Drupal** only:
    * for Drupal to run normally
    * namely stores the Drupal modules':
      * phone subscribers nodes
      * programs nodes.

  * **the Drupal modules**:
    * **AudioSpread Business Manager**
      * manage **the phone subscribers personal details and accounts**:
        * 1 phone susbscriber = 1 _CCK_-modified node
    * **AudioSpread Content Manager**
      * manage **a tree made up of programs tagged with category names**:
        * 1 program = 1 _CCK_-modified node with
        * category tagging thanks to Taxonomy
        * file attachment/uploading thanks to the _fileframework_ module.
      * the tree has a "minimals" category containing:
        * subcategories for each kinds of menu contexts:
          * home,recording,categories,programs,playing,support...
        * all the required audio files/filenames for the phone menus minimal operation.
        * ie. welcome message, category menu info message, programs menu info message, bookmarks info message..
        * Items not overridden will result in picking from:
          1. chosen language pack,
          1. if no such pack, from default language pack.

## AudioSpread Asterisk Server ##
  * a **permanent storage** for **audio files**:
    * menu skeleton files (ie. welcome/bye message, numbers in the chosen language)
    * audio programs' files (ie. program's spoken title and content files).
    * in an easy to encode/decode and light in size format like mp3.
  * a **simplified SQL database** containing only **information from the Drupal's server** database which **matters for the phone** application (see the synchronization section below for details).
  * a running **Asterisk server:**
    * on calls-in, behaves as described extensions.conf, exposing phone menus (IVRs):
      1. let a caller browse by phone through categories and listen to audio programs.
      1. let a caller submit a message that he/she records over the phone.
      1. callback a user if he is premium (should be possible for sms reception (was CORE...)).
  * data used by the Asterisk server are:
    * the categories/programs tree fetched from the local SQL database to expose the menus (IVR) over phone.
    * the needed audio files streamed over phone from the local storage.



## AudioSpread synchronization scripts ##
  * Those scripts are **located and running on both AudioSpread servers**.
  * **Synchronization** has to done in **two directions**:
  * **Drupal => Asterisk**:
    * **mp3 files**
    * **programs/categories tree** (only important details) (with published/unpublished state)
    * **phone subscribers** (only phone-mattering details)
  * **Asterisk => Drupal**:
    * **recorded messages**:
      * caller info
      * two mp3 files (caller's intro & body) for administrator listening & approval before publishing.
    * **durations of calls per caller ID**
  * **Technologies involved**:
    * Triggered on either side by: cron/XML\_RPC.
    * Data synchronization with: XML\_RPC and/or SVN/git/sftp.