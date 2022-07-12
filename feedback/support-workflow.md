# Support Workflow

From speaking with Kirby, it seems that the number of open cases and how to handle stale cases is something that could be iterated on over time.

If I may recommend, setting up filters in ZenDesk can be helpful for the Support Team.

Examples:

* All Active
* Almost Stale 16 Hours
* Stale 24+ Hours
* Stale 7 Days
* etc...

Combined with this, a script or bot to automate what I call the "3 Strikes Process" can be helpful to autoclose pending cases.

> Howdy $NAME,
>
> We have not heard from you in some time. Could you let us know if you have any remaining questions on the issues discussed in this ticket. If not, can we mark this ticket as Resolved?
>
> Cheers,&#x20;
>
> Ross Fisher
>
> Senior Support Engineer - KubeCost **or** KubeCost Support Bot

A total of three messages are sent to the customer before auto-closing, giving the customer ample opportunity to respond. The case, of course, is set to pending each time so it doesn't clog up our active case view.
