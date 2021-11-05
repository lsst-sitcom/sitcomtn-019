..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. note::

   **This technote is not yet published.**

   This proposal outlines how observing tasks will be flowed from the planning stage through execution,
   then linked to data analysis technotes and/or notebooks.

Motivation for this proposal
============================

    * We want central repository for all information related to a task executed at night.
    * We want to have a way to know how and when the tasks were run and how the data was processed.
    * Search and find tasks and their results.

Steps
=====

    * Run planning
        * Which tasks/test are we running during a observation run?
        * Do we want to store these tasks/tests as JIRA Tickets?
        * If using JIRA Tickets, we should use it as a place where we centralize relational information (link to
          notebooks, LOVE scripts configuration files, test cases, confluence log pages, etc.)

    * Nighttime observation
        * Keep the Confluence Logging Page? Yes, for now it is the easiest place for nighttime staff to record task info.
        * Should we add the Confluence Logging Page to the AuxTel Run ticket? Yes!
        * We split the notebooks into task instead of having a single notebook for each observing night or observing
          run. It seems that our team is happy with this new structure since it gives flexibility to run them in a
          different order it desired. These notebooks now live in a private repository on GitHub, which is not ideal.
          However, this new structure might give us a better idea about how do we want to organize them.

    * Daytime JIRA compilation
        * During the daytime, somebody must be resposible for extracting relevant information from nightlog and populating
          the associated JIRA ticket

    * Data Processing
        * Do we want to have the data processing for each task case as a JIRA ticket? And do we want to assign it to
          one or more person as a way of keeping track of which data was processed and how?
        * Data processing falls on the task proposer, the JIRA ticket should be assigned back to the task proposer
          and/or the person who filled the ticket and results/analysis steps should be added to the JIRA ticket.

    * Results
        * This is were we got major agreement: we want to store the tests results (including planning, observing,
          processing) in to technotes (just like this one!).
        * Some tasks may not get to this stage, but only require a short summary of results in the JIRA ticket.

Example JIRA ticket Workflow
============================

   - Stakeholder develops idea for AuxTel nighttime task.
   - Task is proposed at AuxTel pre-planning meeting and adpoted
   - Task is captured in a JIRA task ticket, which may/may not be associated with a top-level ticket for upcoming run/night.
     Ticket is elevated to Proposed.
   - JIRA ticket is reference in observation planning confluence page making it visible to nightcrew (observation planning
     confluence should likewise be linked in JIRA ticket)
   - Nightcrew executes task as described in JIRA ticket, capturing relevant information in nightlog.
   - Daycrew extracts information from nightlog and populates JIRA ticket, linking nightlog. Ticket is elevated to ???
   - Daycrew informs stakeholders/task proposers of ticket and elevates it to Analysis.
   - Stakeholders/task proposers perform analysis and report status back to JIRA ticket or ticket reporter. Ticket is elevated
     to Done.


.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
