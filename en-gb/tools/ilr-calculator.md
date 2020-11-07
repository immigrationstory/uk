---
layout: page
title: ILR Calculator
theme: /public/css/immigrationstory-20200831.css
comments: true
categories: tools
---

Use this tool to calculate when is the earliest date you can apply for Indefinite Leave to Remain, given your continuous qualifying period and the 28-day rule.

* Table of Contents
{:toc}

## Before using the calculator...
![](/assets/ilr-calculator-understand-rules.jpg)

Note that the [official government guidance](https://www.gov.uk/government/publications/indefinite-leave-to-remain-calculating-continuous-period-in-uk) refers to this period in a number of similar but slightly different ways. For the purposes of your ILR application, all of these mean the same thing:

* Qualifying period
* Continuous period
* Continuous qualifying period
* Continuous lawful period
* Continuous residence period

So that this calculator can provide you your eligibility date correctly, you will need to know a number of things:

* What ILR route you are applying for, and what the detailed requirements are for your route
* Most routes require you to have spent a continuous period of five years in the UK; are you in this route, or is it different -- see the section "Categories where the continuous
period is not 5 years" in the official UK Government guidance for [Indefinite leave to remain: calculating continuous period in UK](https://www.gov.uk/government/publications/indefinite-leave-to-remain-calculating-continuous-period-in-uk).
* Most people only have a single grant of leave that covers their entire continuous period; if you have multiple grants of leave that make up the continuous period then ensure you have on-hand the start and end dates are for each of them
* Your date of actual entry into the UK

This tool assumes:

* Your application is straightforward; for instance:
  * You have not been away from the UK for more than 180 full days in any 12-month period
  * You are not applying for other routes such as the 10-year Private Life or Long Residence route
  * You are not applying for asylum
* If you have multiple grants of leave in the continuous period you'd like to be considered for your application, that all of those grants of leave are "compatible" with the ILR route you are applying for and does not reset the clock for your ILR application

If you know that your case is complex then this tool is not for you. You need to reach out to an OISC-registered immigration adviser to assess your situation and provide guidance tailored to your specific circumstances.

The second point above is very important as there are multiple ILR routes that you can take, and each route will have different visas that count towards it, for example:

* The ILR route for partners of British Citizens or those already settled in the UK only counts the grants of leave where you had a Family Visa as a partner or a spouse
* The ILR route for people on a Tier 2 (General) visa only counts the grants of leave where they were on that visa, or a short list of other work-related visas

I personally know many people who have made this mistake. Be careful if you find yourself in a situation where you are changing between visa types!
{: .tip }

If a person then started on a Tier 2 (General) visa and stayed there for four years, and then switched to a Family Visa (say, because they married a British citizen, and they have resigned from their work with their original Tier 2 sponsor), then what happens is that:

* The four years they have spent in the UK under the Tier 2 (General) visa will not count towards the ILR route for partners of British citizens
* The ILR clock resets and starts from zero from the moment they switch their Tier 2 (General) visa to a Family Visa
* The person is no longer eligible to get on the ILR route for people on a Tier 2 (General) visa as they are no longer on that visa

## Calculator: Questionnaire
![](/assets/ilr-calculator.jpg)

<form id="ilr-calculator">
  <div class="box">
  <div class="field">
    <label class="label">What is the length of the continuous period of residence required for the ILR route you are applying for?</label>
    <div class="control">
      <div class="select" required>
        <select name="continuous-period-duration">
          <option value="5">5 Years (most common)</option>
          <option value="3">3 Years</option>
          <option value="2">2 Years</option>
        </select>
      </div>
    </div>
  </div>
  <div class="field">
    <label class="label">When was the start date of your first relevant visa that goes toward the continuous period for this ILR application?</label>
    <div class="control">
      <input class="input" type="date" name="first-visa-start-date" required>
    </div>
    <p class="help">For example, you may have been granted a visa from 2015 to 2018, which you then applied for (and was granted) an extension to 2020. In this case, you had two grants of leave (2015 to 2018, and 2018 to 2020) that you are counting towards your ILR application. Enter the details for the first (earliest) visa above.</p>
  </div>
  <div class="field">
    <label class="label">For the <em>above</em> visa, when was the earliest date that you were physically in the UK; for instance, as indicated by the stamp on your passport?</label>
    <div class="control">
      <input class="input" type="date" name="physical-entry-date" required>
    </div>
    <p class="help">This is important because if you entered the UK late by more than 180 days, then the start of your qualifying period will be your physical entry date, and not the start date of your original visa. If you were already in the UK (for example, because you switched to this visa from a different, unrelated visa) then choose the same date.</p>
  </div>
  <div class="field">
    <label class="label">When is the expiry date of your <em>current</em> visa?</label>
    <div class="control">
      <input class="input" type="date" name="current-visa-end-date" required>
    </div>
  </div>
  </div>
  <div class="field">
    <div class="control">
      <button class="button is-primary is-fullwidth">Calculate</button>
    </div>
  </div>
</form>

### Results

<ul id="ilr-calculator-observations"><li>Click "Calculate" to view results.</li></ul>

<script>

  document.getElementById("ilr-calculator").addEventListener("submit", function(event) {

    const ONE_DAY = 24 * 60 * 60 * 1000;

    event.preventDefault();

    // Inputs

    var firstVisaStartDate = new Date(event.target.elements["first-visa-start-date"].value);
    var physicalEntryDate = new Date(event.target.elements["physical-entry-date"].value);
    var currentVisaEndDate = new Date(event.target.elements["current-visa-end-date"].value);
    var delayedEntry = (physicalEntryDate - firstVisaStartDate) / ONE_DAY;
    var continuousPeriodDuration = parseInt(event.target.elements["continuous-period-duration"].value, 10);

    // Outputs

    var observations = document.getElementById("ilr-calculator-observations");

    // Main logic

    observations.innerHTML = "";
    var observation;

    var qualifyingPeriodStartDate;

    if (delayedEntry > 180) {
      qualifyingPeriodStartDate = physicalEntryDate;
      observation = document.createElement("li");
      observation.appendChild(document.createTextNode("Your physical entry date into the UK is " + delayedEntry + " days (which is more than 180 days) after your leave was granted; thus the start date of your qualifying period for this ILR application is " + qualifyingPeriodStartDate.toLocaleDateString() + " which was when you entered the UK."));
      observations.appendChild(observation);
    } else if (delayedEntry < 0) {
      qualifyingPeriodStartDate = firstVisaStartDate;
      observation = document.createElement("li");
      observation.appendChild(document.createTextNode("Your physical entry date into the UK is earlier than the start date of your relevant grant of leave for this ILR application. We assume this is because you were already in the UK before, but on a different and unrelated visa. Your qualifying period start date is thus " + qualifyingPeriodStartDate.toLocaleDateString() + " which was when your visa was granted."));
      observations.appendChild(observation);
    } else if (delayedEntry == 0) {
      qualifyingPeriodStartDate = firstVisaStartDate;
      observation = document.createElement("li");
      observation.appendChild(document.createTextNode("Your qualifying period start date is " + qualifyingPeriodStartDate.toLocaleDateString() + " which was when your visa was granted, the same date as when you physically entered the UK."));
      observations.appendChild(observation);
      observation = document.createElement("li");
    } else {
      qualifyingPeriodStartDate = firstVisaStartDate;
      var nextYear = new Date(qualifyingPeriodStartDate.getFullYear() + 1, qualifyingPeriodStartDate.getMonth(), qualifyingPeriodStartDate.getDate() - 1);
      var remainingAllowance = 180 - delayedEntry;
      observation = document.createElement("li");
      observation.appendChild(document.createTextNode("You have entered the UK " + delayedEntry + " " + (delayedEntry == 1 ? "day" : "days") + " after the start date of your visa. As this is still within the 180-day allowance, it will be counted as an allowable absence and your qualifying period start date is " + qualifyingPeriodStartDate.toLocaleDateString() + " which was when your visa was granted."));
      observations.appendChild(observation);
      observation = document.createElement("li");
      if (remainingAllowance == 0) {
        observation.appendChild(document.createTextNode("You must ensure that you have not spent any full days outside the UK at all from " + physicalEntryDate.toLocaleDateString() + " to " + nextYear.toLocaleDateString() + " as it will bring you above the 180-day limit of allowed absences in any 12-month period."));
        observations.appendChild(observation);
      } else {
        observation.appendChild(document.createTextNode("You must ensure that you have not been outside the UK for more than " + remainingAllowance + " full " + (remainingAllowance == 1 ? "day" : "days") + " from " + physicalEntryDate.toLocaleDateString() + " to " + nextYear.toLocaleDateString() + " as it will bring you above the 180-day limit of allowed absences in any 12-month period."));
        observations.appendChild(observation);
      }
    }

    var qualifyingPeriodEndDate = new Date(qualifyingPeriodStartDate.getFullYear() + continuousPeriodDuration, qualifyingPeriodStartDate.getMonth(), qualifyingPeriodStartDate.getDate() - 1);
    observation = document.createElement("li");
    observation.appendChild(document.createTextNode("The end date of your qualifying period for this ILR application is " + qualifyingPeriodEndDate.toLocaleDateString() + "."));
    observations.appendChild(observation);

    var earliestIlrApplicationDate = new Date(qualifyingPeriodEndDate.getFullYear(), qualifyingPeriodEndDate.getMonth(), qualifyingPeriodEndDate.getDate() - 28);
    observation = document.createElement("li");
    observation.appendChild(document.createTextNode("Your earliest ILR application date is on " + earliestIlrApplicationDate.toLocaleDateString() + ". Do not submit an application before this date as otherwise your application will be rejected and no refund will be given. If you spend or have spent more than 180 full days in any 12-month period between " + qualifyingPeriodStartDate.toLocaleDateString() + " and " + earliestIlrApplicationDate.toLocaleDateString() + " then your ILR clock will reset and your earliest ILR application date will change. If this is the case then do not use the date above as it is invalid; you must work out the dates given your circumstances or consult with an immigration adviser."));
    observations.appendChild(observation);

    if (earliestIlrApplicationDate > currentVisaEndDate) {
      observation = document.createElement("li");
      observation.appendChild(document.createTextNode("Your current visa expires before your earliest ILR application date. You will need to extend your visa before you can eventually apply for an ILR."));
      observations.appendChild(observation);
    }


  });

</script>

## Further reading
![](/assets/ilr-calculator-further-reading.jpg)

There are dedicated chapters on [preparing for your ILR application]({% post_url en-gb/2020-08-07-prepare-for-ilr %}) as well as the [ILR requirements]({% post_url en-gb/2020-08-08-ilr-requirements %}).

Alternatively, a list of all chapters can be found at the [table of contents]({% link en-gb/table-of-contents.md %}) where you'll find detailed guides on not just applying for Indefinite Leave to Remain, but also other topics such as the process for Tier 2 (General), moving to the UK, living in the UK, and others.

{% include report-issue.html %}