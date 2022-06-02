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

   This proposal outlines how observing tasks are currently flowed from the planning stage through execution, then linked to data analysis technotes and/or notebooks.

   The process is analogous to what is done in the LVV project, where Jira is used as a central workflow management tool, linking the pieces together, monitoring the process,
   and making it visible to all the stakeholders.

   This process will go through a major overhaul in the coming months, but this document acts as the central point for discussion.

Introduction
============

The coordination of night time and daytime activities is continuing to increase as we continue to progress into the commissioning phase.
This document outlines how Auxiliary Telescope observing runs are currently being conducted, from the observation planning stage, through the data reduction phase, and including the impacts on day crews providing critical troubleshooting of issues that have arisen during the night.

Some of the overarching goals are:

- Have night plans point to tasks with practiced and vetted procedures
- Create a central repository for all information related to a task executed at night, which then follows to analyses when appropriate
- A strategy to know how and when the tasks were run and how the data was processed.
- Data analysis and results from the data need to be easily related to when the data was taken.
- A searchable archive to and find tasks and their results.

This document describes the state of the system as it is today, with the goal of making steady improvements, to which there will be many!
This technote is designed to be a "living document" that is updated as the process evolves.

Run Planning Blocks and Epics
=============================

At the time of writing, there are currently two runs AuxTel runs per month, each consisting of 3 nights.
Even with the main telescope, it is anticipated that observing runs will be organized in ~2 week stints due to the dynamic evolution of observatory commissioning.
Therefore, we have adopted each month be separated into A and B blocks (e.g. 2022-05A and 2022-05B).
The exact separation date between A and B is not fixed, however, ideally this would fall near the 15th of the month.
There are scenarios in which planners will want to move to the next block either early or late, specifically if significant system functionality is added or removed.
The most common example of this would be the rollout of a new software interval, however, one could imagine a hardware change (or addition) could be sufficient to warrant a new block.
 
The rollout dates for these cycles are dependent upon when features are needed and/or ready, and therefore the dates are not predictable, nor are hardware schedules.
Therefore we anticipate the the transition to the B block might occur either earlier or later.
In some situations, a C-block may be necessary. 
This is permitted, but should be avoided whenever possible.

Each observing block gets its own SITCOM Jira epic, with a title containing the Observing Block name (e.g. ``2022-06A AuxTel Observing``).
The epic description must contain the dates for which the epic is valid, which is set by the ``Start date`` and ``End date`` parameters. 
Specifics to the run, such as filter changes or any major system changes, should be listed in the epic.
Most of the coordination and system details regarding run coordination are contained in an associated Confluence page night log, this is discussed in a following section.
Of course, all tasks (or Jira stories) that are planned for execution during this block are to be linked to the epic.



Proposing an Observation Activity
=================================

There are numerous types of observing activities, however, they are often categorized into scientific campaigns (e.g. imaging or spectral surveys) and engineering tasks.

- *Campaigns* are meant to indicate a broad set of observations to study system performance or take new science data.
  They often consist of a survey of spectral and/or imaging data, and the visits are coordinated by the Scheduler.
  Although not required, campaigns are generally multi-night studies and not one-off characterizations.
- *Engineering Tasks* encompass activities that are meant to be close to single-occurrence observations.
  This includes functionality testing and characterization studies (e.g. AOS LUT generation), but can also include discretized science observations that are limited in scope and duration.
  These tasks are generally run in a more manual fashion, either via the execution of SAL scripts via the scriptQueue CSC, or when required, via a Jupyter notebook running off of Nublado.

The proposal of an observing activity is done via a Jira ticketing process.

Jira Ticket Creation
--------------------

The jira ticket for a given observation contains all the information to execute an observation, as well as a justification for the observation.
It is acceptable to attach small files and/or link other documents where applicable, specifically in regards to the justification.
The prioritization of the observation is handled by the commissioning leads, but all tasks must go through proper preparations to maximize on-sky efficiency. 
This is discussed further in the `Observation Preparation`_ section.

When proposing an observation, a jira ticket (story), in the `SITCOM Jira Project <https://jira.lsstcorp.org/projects/SITCOM/issues/SITCOM-310?filter=allopenissues>`_.
Be sure to complete the following steps:

#. Link the epic in which the proposal is to performed
#. Add a brief description of the observation, and a justification.
   Linking supporting documentation is encouraged.
#. Include condition constraints, specifically seeing, transparency, and an evaluation of stability (if appropriate).
#. Include details on any required supporting hardware (e.g. the DIMM must be operational)
#. Set the reviewer to ``pingraham``, *and also them add as a watcher*. 
   The ticket creator can also add themselves as a reviewer if desired.
#. For Scheduler driven observations, a configuration file will need to be generated.
   This will be done under a separate ticket and linked here.
   Contact Tiago Ribeiro for assistance and details.
#. If applicable, create a ticket to analyze and publish the results.
   This should be common practice for all non-campaign observations.
#. Results of any analysis or lessons learned should be either added to the ticket as a comment (if short), or published as SITCOM technotes and linked to the ticket(s).

Observation Preparation
=======================

The amount of preparation required is dependent upon the observation, however, all tests need to be run on the Tucson Test Stand (TTS) to the extent possible.
A `starting guide to using the test-stand <https://confluence.lsstcorp.org/display/LSSTCOM/Tucson+Test+Stand+Start+Guide>`_ has the information necessary to get started.
A comment must be added to the ticket saying when TTS testing has been completed.
It is recognized that not all functionality is available on the TTS and therefore the testing may result in only partial coverage of the observing procedure.
Complete the testing to the extent possible.
The goal is to minimize time loss using available tools.

In most cases, specifically when the observations consist of relatively standard steps (e.g. slew and take image), all steps should be carried out using the scriptQueue via LOVE.
Each step and script configuration should be included in the jira ticket.
Be sure to use code-blocks to keep the spacing correct for the command configurations.

.. note::
  It is also possible to use a notebook where the scripts then get sent to the scriptQueue.
  As LOVE is still undergoing improvements, this method is a nice alternative which is easily editable, allows validation of the script configuration(s), and can still be tested on the TTS.
  However, with improvements to LOVE this method will be discouraged to remove the Nublado environment dependency.

When a notebook is used, it must be linked (or attached) to the ticket, prior to execution.
A link is a convenient option as it allows for updates prior to script execution without having to manually upload to the jira ticket.
It is recommended to have an experienced colleague review the procedure prior to on-sky execution.
If possible and useful to the activity, daytime testing using the AuxTel system can be scheduled once TTS testing has been maximized.

Once a notebook is executed on the summit, it will be attached to the ticket by the observer, the ticket will then be put to "In Review".

Observation Planning Good Practice
==================================

This section contains a list of useful tips which are highly recommended to be adopted when writing the observing sequence(s).

- Put the jira ticket name (e.g. ``SITCOM-302``) in the ``PROGRAM`` field for each LATISS image.

Run and Observation Coordination
================================

For each observing block, a confluence "parent" page is created that contains `links to the logs associated with each observing block <https://confluence.lsstcorp.org/display/LSSTCOM/Night+Logs>`_.
The page for each observing block contains both individual night logs, and an Run Planning summary page.

Run Planning Confluence Page
----------------------------

The run planning page acts as the common place to gather all the pertinent information about the observatory system and the required tasks for the observing block.

It includes:

- A link to the observing block epic, such that users can go back and forth efficiently.
- Information on how observers should setup their local environment to be consistent with what is deployed (specifically with the scriptQueue CSC).
- A summary of the system status, with links to tickets where faults are expected that need to be logged.
- A "Calendar" showing who will be supporting the run, and from where.
- A list of outstanding daytime activities which should be completed before the start of the run
- Links to all the engineering tickets
- Links to all the applicable science tickets
- A plan for each night.

Note that this page contains the *intended* plan, and not what was actually executed.
Users must consult the `Night-Specific Confluence Page`_ for those details.



Night-Specific Confluence Page
------------------------------

In the absence of the Rubin Observing Log tool being ready for use, each night has a dedicated observing log page.
It contains the events that occur throughout the night, including when each ticket gets observed.
The executed tickets are be linked in that page.

The page also includes a night summary, including a quantitative breakdown of the time-on-sky etc.


Night Crew Tasks
================

The night crew generally arrives to the summit around 16:00 CLT. 
A pre-night meeting with the run manager and remote support happens at 17:00 CLT.
After this, the night crew will head for supper, depending on the time of year.

The night crew performs numerous tasks, some which include:

- Execute the Daily Checkout script 
- Perform calibrations
- Execute the plan
- Log when data was taken
- Update Jira tickets when data was taken, and link to logs where appropriate
- File fault tickets in the `OBS Jira Project <https://jira.lsstcorp.org/projects/OBS/issues/OBS-35?filter=allopenissues>`_.


Day Crew Tasks
==============

Daytime tasks to support observing, such as responding to faults that will cause time loss the following night, are currently coordinated in the #rubinobs-daytime-tasks Slack channel. 
The run manager is responsible for communicating progress/impacts to the night crew.

Data Reduction and Analysis Coordination
========================================

It is anticipated that the data processing will occur in tickets across multiple jira projects.
Each ticket that requires an associated analysis must have the analysis ticket created and linked before the observing activity ticket can be closed out.

Tests results should be published as technotes (like this one).
The technote itself should contain links to the appropriate tickets, and the tickets should also be linked to the technote.

Some analysis tasks may not require this level of detail and therefore a short summary of results in the JIRA ticket is sufficient.


    .. * Do we want to have the data processing for each task case as a JIRA ticket?
    ..   And do we want to assign it to one or more person as a way of keeping track of which data was processed and how?
    ..   The processing tickets themselves will probably happen across many Jira projects.
    ..   We might have to adopt this workflow such that once the data is taken the ticket gets assigned to the person to handle the reduction/analysis (if obvious),
    ..   or it gets assigned to the run manager to handle the delegation.
    .. * Data processing falls on the task proposer, the JIRA ticket should be assigned back to the task proposer
    ..   and/or the person who filled the ticket and results/analysis steps should be added to the JIRA ticket.



.. Example JIRA ticket Workflow
.. ============================

..    - Stakeholder develops idea for AuxTel nighttime task.
..    - Task is proposed at AuxTel pre-planning meeting and adopted
..    - Task is captured in a JIRA task ticket, which may/may not be associated with a top-level ticket for upcoming run/night.
..      Ticket is elevated to Proposed.
..    - A procedure/test script is developed such that it is ready for execution by the night crew.
..      Further development should not be required.
..    - JIRA ticket is reference in observation planning confluence page making it visible to nightcrew
..      (observation planning confluence should likewise be linked in JIRA ticket)
..    - Nightcrew executes task as described in JIRA ticket, capturing relevant information in nightlog.
..    - Daycrew extracts information from nightlog and populates JIRA ticket, linking nightlog.
..      Ticket is elevated to "Ready to Observe"
..    - Daycrew informs stakeholders/task proposers of ticket and elevates it to Analysis.
..    - Stakeholders/task proposers perform analysis and report status back to JIRA ticket or ticket reporter.
..      Ticket is elevated to Done.


.. Potential Improvements
.. ======================

.. TBR - this is probably a really long list 

.. - Consider a more specialized workflow for observation planning.
.. Another viable option may be a more active use of labels.

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
