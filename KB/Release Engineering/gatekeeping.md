---
date updated: '2021-03-16T03:55:55-05:00'

---

# Gatekeeping

Gatekeeping

The task of ensuring that a given artifact is viable is gatekeeping.  The gatekeeping action falls inside steps 3-5 above.
Gatekeeping operations are the specific sub-states in the lifecycle of an artifact that must meet some criteria before proceeding to the next state.

Consider the following [[lifecycle]], subsequent additions to list of general states (1-8) found in the [[lifecycle]]:

3.1 Not lintered<br/>
3.2 Not compiled<br/>
3.3 Untested<br/>
3.4 Insufficiently Tested<br/>
3.5 Not packaged<br/>

In this case, we could say that for step 3.1 to be passed, the linter for the code could be passed and thus the contents of the code could be eligible for compilation.  Once 3.2 is passed, the code has been "compiled" (whatever that means).  Then tests (3.3)  might be run. If, and only if, they pass would we determine if they were sufficient to proceed (3.4).  If they were sufficient, then the process would move on to the packaging step to produce some potentially useful artifact that could move through some possible additional gatekeeping to move to step 4 above.

If 3.1, 3.2, and 3.3 passed, but 3.4 did not, then the code never made it past the state of being insufficiently tested.  It _remains_
insufficiently tested, and therefore is not eligible for packaging and thus cannot enter the "not packaged" state because it didn't pass the 3.4 gatekeeping operation.  As a "not packaged" thing, it never reached its [[packaging#End State|end state]] and cannot be identified as an [[artifact]].

Now, consider a document written in Google Drive by someone.  It contains some content, so it has reached step 3.

3.1 Not edited<br/>
3.2 Not reviewed<br/>
3.3 Unaccepted<br/>
3.4 Not packaged<br/>

The text might have been speed-typed into the document.
It would then need to pass "editing" and "Review" and the "Acceptance" to be assigned a version in the Google Document version history.
This would constitute "packaging" it for outside use.

Note that this lifecycle is not how non-engineers typically do their work.  It _IS_, however, the prescribed method for actually producing useful documents.  Further, it does not require one to export copies and rename them with new dates to pass around in emails like savages.
