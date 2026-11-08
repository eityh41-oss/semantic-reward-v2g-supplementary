# Language-interface study: the 52 operator texts and outcomes

The test set contains 52 simulated operator texts across nine expression types: 40 specify an unambiguous priority order among voltage security, charging service, and purchase cost, and 12 contain missing information or contradictory requirements. Expected output: the priority order, or a clarification request for incomplete or contradictory texts. The Writer column records the three independent LLM-driven text sources (A, B, C). Interfaces: keyword parser (v0), keyword parser with clarification fallback (v1), qwen-turbo (initial run) and qwen3.8-flash (repeat), both at temperature 0 with three requests per text; the LLM received the system prompt and the verbatim text only.

In the table, V, S, and C denote voltage security, charging service, and purchase cost; for example, V > S > C ranks voltage first, service second, and cost last.

| id | Type | Writer | Expected | Parser v0 | Parser v1 | qwen-turbo | qwen3.8-flash |
|----|------------------|-----|---------|---------|---------|---------|---------|
| r01 | Negation or concession | A | V > S > C | invalid | clarify | V > S > C | V > S > C |
| r02 | Multi-party ruling | C | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r03 | Cross-sentence reference | A | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r04 | Contradiction (should clarify) | C | clarify | V > S > C | clarify | clarify | clarify |
| r05 | Cross-sentence reference | C | C > S > V | clarify | clarify | C > S > V | C > S > V |
| r06 | Standard wording | A | V > S > C | V > S > C | V > S > C | V > S > C | V > S > C |
| r07 | Standard wording | A | V > S > C | clarify | clarify | V > S > C | V > S > C |
| r08 | Superseded instruction | C | V > S > C | clarify | clarify | V > S > C | V > S > C |
| r09 | Standard wording | A | C > S > V | C > S > V | C > S > V | C > S > V | C > S > V |
| r10 | Multi-paragraph handover | B | V > S > C | clarify | V > S > C | V > S > C | V > S > C |
| r11 | Background + final directive | A | C > S > V | C > S > V | C > S > V | C > S > V | C > S > V |
| r12 | Superseded instruction | A | V > S > C | invalid | V > S > C | V > S > C | V > S > C |
| r13 | Contradiction (should clarify) | A | clarify | clarify | clarify | clarify | clarify |
| r14 | Cross-sentence reference | B | C > S > V | clarify | clarify | C > S > V | C > S > V |
| r15 | Missing priority (should clarify) | A | clarify | invalid | clarify | clarify | clarify |
| r16 | Negation or concession | B | V > S > C | V > S > C | clarify | V > S > C | V > S > C |
| r17 | Multi-party ruling | A | C > S > V | clarify | clarify | C > S > V | C > S > V |
| r18 | Superseded instruction | B | V > S > C | V > C > S | V > S > C | V > S > C | V > S > C |
| r19 | Negation or concession | B | C > S > V | invalid | C > S > V | C > S > V | C > S > V |
| r20 | Multi-paragraph handover | A | V > S > C | invalid | V > S > C | V > S > C | V > S > C |
| r21 | Cross-sentence reference | C | V > S > C | clarify | clarify | V > C > S | V > S > C |
| r22 | Superseded instruction | A | C > S > V | clarify | clarify | C > S > V | C > S > V |
| r23 | Background + final directive | B | V > S > C | V > S > C | V > S > C | V > S > C | V > S > C |
| r24 | Negation or concession | A | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r25 | Multi-paragraph handover | B | C > S > V | C > S > V | clarify | C > S > V | C > S > V |
| r26 | Background + final directive | B | C > S > V | clarify | clarify | C > S > V | C > S > V |
| r27 | Multi-paragraph handover | C | V > S > C | clarify | V > S > C | V > S > C | V > S > C |
| r28 | Negation or concession | C | V > S > C | invalid | clarify | V > S > C | V > S > C |
| r29 | Cross-sentence reference | A | V > S > C | clarify | clarify | V > S > C | V > S > C |
| r30 | Multi-party ruling | B | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r31 | Multi-paragraph handover | C | C > S > V | C > V > S | C > S > V | C > S > V | C > S > V |
| r32 | Missing priority (should clarify) | C | clarify | clarify | clarify | S > V > C | clarify |
| r33 | Missing priority (should clarify) | B | clarify | invalid | clarify | V > S > C | clarify |
| r34 | Superseded instruction | C | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r35 | Missing priority (should clarify) | C | clarify | clarify | clarify | V > S > C | clarify |
| r36 | Cross-sentence reference | B | V > S > C | invalid | clarify | V > S > C | V > S > C |
| r37 | Superseded instruction | B | C > S > V | clarify | C > S > V | C > S > V | C > S > V |
| r38 | Multi-paragraph handover | A | C > S > V | invalid | clarify | C > S > V | C > S > V |
| r39 | Multi-party ruling | A | V > S > C | invalid | clarify | V > S > C | V > S > C |
| r40 | Multi-party ruling | C | V > S > C | V > S > C | clarify | V > S > C | V > S > C |
| r41 | Missing priority (should clarify) | A | clarify | clarify | clarify | clarify | clarify |
| r42 | Background + final directive | A | V > S > C | V > S > C | clarify | V > S > C | V > S > C |
| r43 | Contradiction (should clarify) | A | clarify | invalid | clarify | clarify | clarify |
| r44 | Contradiction (should clarify) | C | clarify | V > C > S | clarify | clarify | clarify |
| r45 | Contradiction (should clarify) | B | clarify | clarify | clarify | V > C > S | clarify |
| r46 | Contradiction (should clarify) | B | clarify | clarify | clarify | V > S > C | clarify |
| r47 | Multi-party ruling | B | V > S > C | invalid | clarify | V > S > C | V > S > C |
| r48 | Negation or concession | C | C > S > V | clarify | C > S > V | C > S > V | C > S > V |
| r49 | Background + final directive | C | C > S > V | clarify | C > S > V | C > S > V | C > S > V |
| r50 | Missing priority (should clarify) | B | clarify | invalid | clarify | S > V > C | clarify |
| r51 | Standard wording | A | C > S > V | C > S > V | C > S > V | C > S > V | C > S > V |
| r52 | Background + final directive | C | V > S > C | invalid | V > S > C | V > S > C | V > S > C |

## Texts

### r01 (Negation or concession, writer A, expected: voltage > service > cost)

I want to be clear about what this evaluation is not. It is not a cost-minimisation exercise, and it is not mainly about pleasing drivers either. Voltage comes before anything else: a dispatch that keeps every bus comfortably within its limits beats one that does not, even if the safer one leaves some vehicles less well served. Paying somewhat more for electricity is acceptable if that is what it takes to give drivers better service, shorter waits at low charge and fuller charge at departure, because service ranks above the purchase bill. Do not read this as permission to ignore cost; it still matters, just less than the other two. And do not let service gains justify pushing voltages toward the edge, because that trade goes the other way.

### r02 (Multi-party ruling, writer C, expected: cost > service > voltage)

Three views came in on the evaluation criteria. The grid engineer asked that keeping bus voltages within limits be the main test of a good dispatch day. The fleet coordinator would prefer that vehicles be served first, with minimal waiting at low charge and no one leaving short of what they requested. The finance lead asked that electricity purchase cost be the headline measure. I have the final say on this, and after weighing all three, I am siding with finance: cost is the leading criterion. Of the two remaining, the fleet coordinator's service concern ranks above the voltage question. Voltage stays in the evaluation as the final consideration, so the grid engineer's request is not discarded, only placed behind the others. That ruling holds for the whole day.

### r03 (Cross-sentence reference, writer A, expected: cost > service > voltage)

Let me lay out the three concerns before ranking them. One is keeping bus voltages within their limits. Another is driver service at the chargers, covering both time spent waiting at low charge and departing short of the requested energy. The last is what we pay for electricity bought from upstream. For today's whole-day evaluation, the last of these is the one that matters most. The second one I mentioned follows it. The one I listed first is still part of the judgment, but it ranks below the other two. Put simply, the order I want is the reverse of the order in which I introduced them.

### r04 (Contradiction (should clarify), writer C, expected: clarify)

Here is the evaluation guidance for the day. Voltage security is the top priority: keeping every bus within its limits matters more than anything else in the assessment. Service to vehicles comes next, with short waits at low charge and departures with the requested energy. Electricity purchase cost is the least important of the three. At the same time, and this is equally firm, the purchase cost of electricity must be the leading criterion today, ahead of both service and voltage, because the budget review depends on it. Both of these instructions come from me and both stand; please apply them together across the whole day.

### r05 (Cross-sentence reference, writer C, expected: cost > service > voltage)

When reviewing a dispatch day, you'll be weighing voltage security on the feeder against the quality of charging service, and both against what the electricity cost us. Start from the last of those. It is the one I care about most, and a day that buys energy expensively should not score well just because it looks good on other fronts. As for the first two, the latter takes precedence over the former: vehicles waiting long at low charge, or departing short of their requested energy, is a bigger concern than voltages drifting toward their limits. That is not to say the former can be ignored; it remains part of the judgement, simply at the bottom of it.

### r06 (Standard wording, writer A, expected: voltage > service > cost)

Voltage security comes first, EV charging service second, and electricity purchase cost last.

### r07 (Standard wording, writer A, expected: voltage > service > cost)

Prioritise keeping bus voltages within limits, then EV charging service, then electricity purchase cost.

### r08 (Superseded instruction, writer C, expected: voltage > service > cost)

Please disregard the evaluation guidance circulated earlier this month. That note asked for dispatch to be judged mainly on how cheaply the energy was bought, with driver service second and the grid last. It has been withdrawn in full after the review meeting and should not be used for anything going forward. The arrangement that replaces it is as follows. Across the whole day, voltage security comes first: a dispatch that keeps every bus within its voltage limits is better than one that does not, before anything else is considered. Charging service for the vehicles is the next consideration, covering both how long cars sit waiting while their batteries are low and whether they depart with the charge they requested. Electricity purchase cost is the least important consideration, though it is not to be ignored.

### r09 (Standard wording, writer A, expected: cost > service > voltage)

Minimise electricity purchase cost above all, then serve EV charging needs, and treat voltage security as the lowest priority.

### r10 (Multi-paragraph handover, writer B, expected: voltage > service > cost)

Shift handover, night to day.

Overnight was quiet. Two chargers at the north hub faulted and were reset; both are back in service. No customer complaints were logged.

Yesterday afternoon we were told to treat cost as the top priority while the price review was running. That review finished last night and the control room supervisor cancelled the instruction, so it does not carry over.

The coming day is evaluated under the supervisor's standing guidance for this feeder. The day is judged first on voltage security, keeping all bus voltages within limits. Charging service is the next consideration: avoid long waits for vehicles at low charge and make sure each car leaves with its requested energy. Electricity purchase cost is the last of the three.

The forecast is cloudy, so expect little help from rooftop solar.

### r11 (Background + final directive, writer A, expected: cost > service > voltage)

For the record, the utility's wholesale contract was renegotiated this quarter, and our upstream purchase price has become the line item management reviews every week. The feeder itself has been comparatively healthy lately; voltage complaints have been rare and the engineers describe the margins as comfortable. Station attendants report that charging customers are mostly satisfied, though some still mention long stretches at low battery. With that in mind, my instruction for evaluating the whole day is this: what we pay for electricity from upstream is the primary concern. Charging service for the vehicles, meaning not leaving them waiting while nearly empty and not sending them off without the energy they requested, ranks next. Voltage security still counts, but it sits at the bottom of the list for today's judgment.

### r12 (Superseded instruction, writer A, expected: voltage > service > cost)

Please disregard the guidance I circulated yesterday. In that note I said purchase cost should lead and that voltage could be treated as the last consideration; that arrangement is withdrawn and should not be applied to anything you evaluate from now on. After talking it over with the network planning group, the current arrangement is different. Voltage security on the feeder takes precedence over everything else in this assessment. Charging service for EV owners, that is, how long they sit with a low battery and whether they depart with the charge they asked for, is the second consideration. Electricity purchase cost comes after both of those. Judge the full day's dispatch on that basis only, and treat the earlier note as void.

### r13 (Contradiction (should clarify), writer A, expected: clarify)

Handover for the evaluation desk. To be clear about the current ordering of our three concerns: charging service for the vehicles matters more than electricity purchase cost, and purchase cost in turn matters more than keeping bus voltages within limits. Voltage security, meanwhile, matters more than charging service. All three of these comparisons apply at the same time to the whole day, and none of them is a past arrangement that has been withdrawn. Please judge today's dispatch so that every one of these statements is respected exactly as written, without dropping any of them.

### r14 (Cross-sentence reference, writer B, expected: cost > service > voltage)

Consider the three things this day will be judged on: what we pay for electricity, the charging service drivers receive, and whether bus voltages stay within limits. Of these, the one listed first is tomorrow's main criterion. The second item, which covers both how long a car waits at low charge and whether it leaves with the energy requested, comes right after it. The last item on that list is the least important of the three. It should not be ignored, but where it competes with either of the others, it gives way. In short, the item I mentioned first beats the one I mentioned second, and that one in turn beats the third.

### r15 (Missing priority (should clarify), writer A, expected: clarify)

For today's evaluation, voltage security on the feeder clearly comes first; nobody disputes that. What I have not settled is how charging service and electricity purchase cost compare with each other. Finance and customer relations each make a fair case, one about the purchase bill and the other about drivers waiting at low charge or leaving short of what they booked, and I am still waiting for direction from the regional office before deciding which of the two should rank higher. Until then I am not giving a ranking between them. Please do not assume one; both matter, but their relative importance is open for now.

### r16 (Negation or concession, writer B, expected: voltage > service > cost)

To be clear about tomorrow's priorities: this is not a cost-saving exercise. Paying somewhat more for electricity is acceptable if that is what it takes. Nor is driver service the thing we put before everything else. What must be avoided above all is a schedule that pushes bus voltages outside their limits; protecting voltage is the first thing the dispatch will be judged on. That said, drivers are not an afterthought. Of the remaining concerns, service matters more than the electricity bill, so cars should not sit at a low battery for long, and they should not leave without the charge they requested, even if keeping that promise costs us a bit extra. Cost comes last, though I would still rather not waste money needlessly.

### r17 (Multi-party ruling, writer A, expected: cost > service > voltage)

Three views came in on how to evaluate the day. Grid operations said keeping bus voltages within limits should dominate. The charging-network team argued for putting drivers first: short waits when batteries are low and full delivery of requested charge. Procurement asked that electricity purchase cost lead, since the budget is under pressure. I have heard everyone, and as the shift supervisor I am making the call. Procurement's position wins: purchase cost is the top priority. The charging-network team's concern comes next, ahead of grid operations' request. Voltage limits remain part of the evaluation but matter least of the three. Please apply this ruling, not any individual team's wish.

### r18 (Superseded instruction, writer B, expected: voltage > service > cost)

Update to the dispatch guidance. On Monday I circulated a note asking you to put purchase cost ahead of everything else, with drivers second and the network last. Please disregard that note entirely; it was written before the substation team reported how weak the end of the feeder is under evening load, and it no longer applies. The current guidance for judging a full day of charging is this. Voltage security is the primary concern, so the schedule should keep bus voltages within their limits. Next comes the service we give to EV owners: short waits when a car arrives nearly empty, and full delivery of what each driver requested before departure. Electricity cost is now the lowest of the three priorities. I still want it watched, but only after the first two are taken care of.

### r19 (Negation or concession, writer B, expected: cost > service > voltage)

I want to head off a misunderstanding about how tomorrow will be judged. It is not voltage first this time. The feeder can tolerate a few modest excursions, and we are willing to accept a somewhat weaker voltage profile if that is the price of the other goals. It is also not service above all else. The main measure is how much we pay to buy electricity across the day; keeping that down comes first. Customer service is not being dropped either: between service and voltage, service is clearly the more important, so avoid leaving vehicles at low charge for long and avoid sending them away short of their requested energy. Voltage comes after both. It is still worth keeping reasonable, but it gives way when it conflicts with savings or service.

### r20 (Multi-paragraph handover, writer A, expected: voltage > service > cost)

Handover note, day desk.

Overnight was quiet. The night team logged a brief alarm on one of the far-end buses, which cleared without action; no equipment faults remain open.

Station staff mentioned that a few drivers complained about sitting at very low charge for a long time, and one left without the energy she had booked. Customer relations would like this kept in view.

For today's evaluation, the standing task is unchanged from what the planning group set: voltage security on the feeder is the leading criterion. Charging service for the vehicles is next, ahead of electricity purchase cost, which is the least of the three.

Finance asked whether cost could be moved up; that request was not approved, so keep the order above.

### r21 (Cross-sentence reference, writer C, expected: voltage > service > cost)

Three things get weighed when a day of dispatch is reviewed: the condition of the feeder, meaning whether bus voltages stay within their limits; how much we pay for power; and how well the vehicles are charged, meaning no long waits at low battery and no cars leaving without their requested energy. The first of these is the main concern, and it outranks everything else when you assess a day. The other two are both secondary to it, but they are not equal: the third matters more than the second, so a cheaper purchase bill should not excuse poor charging. In other words, after the feeder comes the item I named last, and the one I named in the middle sits at the bottom.

### r22 (Superseded instruction, writer A, expected: cost > service > voltage)

Correction to the standing instruction. Until this morning the desk was told to rank feeder voltage above everything, with service in the middle and cost trailing behind. That instruction has been cancelled by the operations manager and no longer applies. The replacement is as follows. Our main yardstick for the whole day is the cost of the electricity we buy from the upstream grid; a dispatch that keeps that bill down is the better dispatch. Next in importance is the service the chargers give to drivers, meaning they should neither be left waiting at a low state of charge nor leave without the charge they requested. Keeping bus voltages within limits is now the lowest of the three, though it is not something to neglect entirely.

### r23 (Background + final directive, writer B, expected: voltage > service > cost)

Some context before tomorrow's plan. The feeder has been carrying heavy evening residential load all week, and with very little solar on the far laterals, the tail-end buses have been sagging every night once commuters plug in. Procurement also mentioned that wholesale prices have crept up, and the charging hub keeps forwarding complaints from drivers. None of that changes the basic picture, so here is how I want tomorrow's whole-day dispatch judged. Keeping every bus voltage inside its limits comes first. After that, look after the drivers: nobody should sit for a long time on a low battery, and nobody should drive away without the charge they asked for. What we pay for electricity matters least of the three; it still counts, but it should not be bought at the expense of the grid or the drivers.

### r24 (Negation or concession, writer A, expected: cost > service > voltage)

This is not a voltage-first day, and nobody should judge it as one. The feeder can live with somewhat weaker voltage margins if that is the price of a cheaper or better-served day; voltage is the last of our three concerns, not an absolute veto. Nor is driver service the headline. What I care about most is not overpaying for the electricity we purchase. If a schedule saves money at the expense of slightly longer low-battery waits or a little less energy delivered before departure, that is a trade I accept. Still, service is not irrelevant: when costs are comparable, the schedule that serves vehicles better wins, and service should not be sacrificed merely to tidy up voltages.

### r25 (Multi-paragraph handover, writer B, expected: cost > service > voltage)

Handover note, evening shift.

The substation crew finished their maintenance at four and the feeder is back in its normal configuration. They report healthy voltage margins on all laterals.

From operations: for the coming day, judge the charging schedule mainly on the money spent buying electricity. That is the headline concern from the commercial side and it has been agreed with the shift lead.

The customer desk asked me to pass on that they still care about service, and the shift lead confirmed it ranks right behind cost: drivers should not wait long on a low battery and should not leave short of what they requested.

On voltage, the shift lead's view is that with margins this healthy it is the least pressing of the three. Keep it in view, but it should not override cost or service.

Nothing else outstanding.

### r26 (Background + final directive, writer B, expected: cost > service > voltage)

For background: last quarter the finance team flagged that our upstream energy purchases ran well over budget, mostly because charging bunched up during expensive hours. The network side has been quiet. The feeder has had spare headroom on most days, and voltage excursions have been rare and mild. Drivers have generally been satisfied with the hub. With that in mind, the instruction for the coming day is as follows. Judge the dispatch first on how little we spend buying electricity. The second consideration is charging service, meaning vehicles should not be left waiting at a low state of charge for long and should depart with the energy they requested. Voltage security sits at the bottom of the list for this day: keep an eye on it and avoid needless excursions, but do not trade away cost savings or customer service to polish voltage margins.

### r27 (Multi-paragraph handover, writer C, expected: voltage > service > cost)

Handover notes, control room.

Equipment: the capacitor bank at the far end of the feeder was inspected this morning and is back in service. No outstanding alarms.

Charging hubs: steady demand all day, nothing unusual. A few chargers were reported slow by drivers; maintenance has a ticket open.

Evaluation task carried over from the supervisor: the dispatch for the whole day should be judged first on voltage security, keeping every bus inside its limits. After that comes the charging service given to vehicles: avoid long waits with low batteries, and make sure they leave with the charge they asked for. Behind both sits the cost of the electricity we buy, which should be kept in mind but does not outrank the other two.

Nothing else pending. The same task applies to the next shift; please confirm on arrival.

### r28 (Negation or concession, writer C, expected: voltage > service > cost)

Before anyone scores the day, a word on what this evaluation is not about. Cost is not the first thing to optimise, and squeezing every last bit of convenience out of the charging sessions is not the main goal either. A higher electricity bill is a price I am willing to pay to keep the network healthy, and drivers can tolerate a slightly less ideal session for the same reason. The first thing I will judge is whether the dispatch kept bus voltages within their limits; nothing ranks above that. Between the remaining two, the drivers come before the bill: long waits on a low battery, or vehicles leaving short of their requested charge, matter more than a higher purchase cost. Cost is still tracked, just at the bottom.

### r29 (Cross-sentence reference, writer A, expected: voltage > service > cost)

Two things keep coming up when we review dispatch: what we spend on electricity, and how well drivers are served at the chargers, which covers both avoiding long waits at low charge and making sure they leave with the charge they requested. Between these two, the latter matters more to us than the former. There is a third concern that outranks both of them, and that is voltage security on the feeder, keeping every bus within its limits. This point should guide the evaluation above all else. So when you judge the day, start from this point, then look at the service matter I described, and only after that consider the spending.

### r30 (Multi-party ruling, writer B, expected: cost > service > voltage)

Three views were put forward at the planning meeting. Grid operations asked for voltage to be the overriding priority. The hub manager asked that customers be protected above everything else. The commercial team wanted the day judged mainly on what we spend on electricity. The regional director, who has the final say, closed the discussion with the following decision. For the whole of tomorrow, purchase cost is the top criterion. The hub manager's point comes next: service to EV drivers, meaning short waits at low charge and departures with the requested energy, ranks second. Grid operations' request is not adopted as the lead priority; voltage security ranks third. It still counts and the network should not be neglected, but within this evaluation it is the least important of the three.

### r31 (Multi-paragraph handover, writer C, expected: cost > service > voltage)

Shift handover, overnight crew to day crew.

Weather was mild and the feeder was lightly loaded. Voltage readings stayed comfortably within range; the monitoring team has no concerns to pass on.

Finance stopped by and reiterated what the operations director decided last week, which remains in force: when we judge a day's charging dispatch, the electricity purchase cost is what counts most. Driver service comes after cost, so keep waiting times short for vehicles that arrive nearly empty and make sure cars leave with the charge they requested. Voltage limits are the remaining item, still assessed but ranked behind both cost and service.

The car park barrier is sticking again; facilities have been told.

That's everything. Carry the same evaluation criteria through the full day.

### r32 (Missing priority (should clarify), writer C, expected: clarify)

Quick note on how we'll be judging dispatch. Several people raised the charging experience, and I agree it should count for more than voltage: a day where vehicles wait long at low charge or leave short of their requested energy is worse than one where bus voltages run closer to their limits. Voltage is still part of the review, of course. The electricity bill will also be looked at, as it always is. Where it fits relative to the other two hasn't been decided; procurement and the network team are still arguing it out and there's no ruling yet. Apply this to the whole day.

### r33 (Missing priority (should clarify), writer B, expected: clarify)

A quick note on tomorrow's charging dispatch. The feeder has been under some strain in the evenings, so I want voltage security to be treated as more important than the electricity bill when the two conflict. Bus voltages should stay inside their limits. Please also remember the drivers: we have had a few complaints about cars sitting at a low battery for a long time and about vehicles leaving with less charge than they asked for. Service matters to us and should be part of how the day is assessed. Use your judgement on the rest and send me the schedule once it is ready.

### r34 (Superseded instruction, writer C, expected: cost > service > voltage)

Update to the dispatch evaluation criteria. Until today we had been judging charging schedules primarily on the experience of the drivers, with voltage limits as the next concern and the electricity bill treated as an afterthought. That arrangement is cancelled as of this message; please don't carry any of it forward. The new criteria are these. The cost of purchasing electricity is now what matters most when comparing one day's dispatch with another. Service to the vehicles, meaning short waits when batteries are low and departures with the requested charge, sits in the middle. Keeping bus voltages inside their limits is the lowest of the three. It still belongs in the evaluation and we are not dropping it, but it yields to both of the others.

### r35 (Missing priority (should clarify), writer C, expected: clarify)

For today's dispatch evaluation, the main thing is the feeder. Bus voltages need to stay within their limits, and any schedule that lets them stray should be judged harshly, ahead of anything else we look at. Beyond that, I'd like the assessment to also reflect how the drivers were served, meaning whether vehicles waited long at a low state of charge and whether they left with what they asked for, and what we ended up paying for electricity. Both of those belong in the evaluation and both should be taken seriously. I haven't settled in my own mind which of them deserves more weight, and I'd rather not guess, so I'm leaving that open for now. Please use this for the whole day.

### r36 (Cross-sentence reference, writer B, expected: voltage > service > cost)

Two things tend to pull against each other on this feeder in the evening: keeping bus voltages within their limits, and keeping the electricity bill down. For tomorrow, the former clearly outranks the latter. There is a third concern as well, namely the service drivers get at the charging points: not waiting a long time with a nearly empty battery, and leaving with the charge they asked for. This point sits between the two I named first. It yields to the one that outranks, and it takes precedence over the one that is outranked. So whenever the schedule has to give something up, the bill is what gives first, and the network is what gives last.

### r37 (Superseded instruction, writer B, expected: cost > service > voltage)

Please note a change of instructions. The guidance issued at the start of the month, which put customer service at the top, voltage second and price last, has been withdrawn by the operations manager and should not be used for any further evaluation. That arrangement made sense during the launch promotion, which is now over. From today, a day of charging dispatch is to be assessed with electricity purchase cost as the leading criterion. Charging service follows in second place: we still do not want vehicles stuck at low charge for long periods, and we still want them to leave with what they asked for. Holding bus voltages within limits is the third and least important concern under the new guidance. It is not to be ignored, but it yields to the other two.

### r38 (Multi-paragraph handover, writer A, expected: cost > service > voltage)

Shift handover, dispatch evaluation.

Network status: no outages, voltage readings have been within the normal range for most of the week, and maintenance has nothing scheduled.

Customers: the charging desk passed on some feedback about cars waiting at low battery and departing without their full requested charge. They want service treated as an important item, not a forgotten one.

Current task: management confirmed this morning that electricity purchase cost is the main criterion for judging the whole day. Charging service comes after cost but before voltage. Voltage security is the lowest of the three priorities, though it is still included.

Reminder: an old sticky note on the console still says 'voltage first'. It predates this instruction and should be ignored.

### r39 (Multi-party ruling, writer A, expected: voltage > service > cost)

We met this morning to agree how today's dispatch should be judged. The finance representative argued that purchase cost should come first, given the recent price increases. The customer-relations lead wanted charging service at the top, pointing to complaints about cars stuck at low charge and drivers leaving without the energy they booked. The network engineer insisted that voltage problems on the feeder are the real risk. As duty manager, the decision is mine, and here it is: voltage security leads. Customer service comes second; I take the complaints seriously, and they outrank the bill. Purchase cost comes third, and finance's concern is noted but yields to the other two. That is the ruling for the whole day.

### r40 (Multi-party ruling, writer C, expected: voltage > service > cost)

We had a long discussion about how to score a day of dispatch. Priya from procurement argued that purchase cost should lead, since that is what the monthly review looks at. Tom from customer operations wanted driver service at the top: short waits for low-battery vehicles and full delivery of the requested charge. Lena from network planning pointed out that the feeder's voltage margins are thin and should not be traded away. As the manager responsible for this evaluation, here is my decision, and it is the one that applies. Voltage security on the feeder comes first. Tom's concern about driver service comes next, and I agree it is important. Priya's point about cost is taken on board, but cost ranks last of the three. Please judge every full day of dispatch on that basis.

### r41 (Missing priority (should clarify), writer A, expected: clarify)

At the coordination meeting, the network engineer argued that keeping bus voltages within limits must lead the whole-day evaluation, with driver charging service next and purchase cost last. The procurement lead disagreed sharply and wanted electricity purchase cost at the top, with service second and voltage last. The customer team took no side on the top item, only saying that drivers should not wait long at low charge or leave short of their requested energy. The meeting ended without a decision; the duty manager was absent, and nobody else present has the authority to choose between the two positions. The minutes record both views and note that a ruling is still pending.

### r42 (Background + final directive, writer A, expected: voltage > service > cost)

Some context before the instruction. Last month the feeder saw several evenings where residential demand climbed while rooftop solar had already faded, and the far end of the lateral sagged noticeably. Our protection engineers flagged it, and the regional office has asked every dispatch desk to take it seriously. At the same time, drivers keep telling the station staff that they hate sitting at a nearly empty battery or driving away with less charge than they asked for, and the finance team has been watching our purchase bills closely. Here is how I want today's dispatch judged. Keeping bus voltages inside their limits is the thing that matters most. After that, look at how well the vehicles are served: short waits at low charge, and leaving with what they requested. Electricity purchase cost is the least important of the three, though it should not be thrown away for nothing.

### r43 (Contradiction (should clarify), writer A, expected: clarify)

Here is the current ranking for today's whole-day evaluation. Voltage security on the feeder leads, and nothing should be placed above it. Charging service for drivers, meaning short waits at low charge and leaving with the requested energy, ranks second, and the electricity purchase bill comes third. However, please also treat the purchase bill as the top concern of the day, ahead of voltage, because finance has made that the leading criterion. Both statements belong to the same current instruction; neither replaces the other, and I expect the dispatch to be judged by both at once.

### r44 (Contradiction (should clarify), writer C, expected: clarify)

On judging the charging dispatch: the drivers' experience outranks the electricity bill. Long waits with low batteries and cars departing short of their requested charge are worse than paying more for power. Voltage within limits sits below both of those. That is the standard for the whole day. Also, to be clear, for the same day and the same evaluation, keeping the bus voltages within limits should be the most important factor, and purchase cost should rank ahead of how the vehicles are served. I'm not withdrawing either part; treat everything in this note as current.

### r45 (Contradiction (should clarify), writer B, expected: clarify)

Instructions for tomorrow's whole-day dispatch. The commercial position this month is tight, so the schedule should be judged first and foremost on how little we spend buying electricity; nothing else should come ahead of that. Driver service is the second consideration: no long waits at low charge, and cars should leave with the energy they asked for. One more thing from the same review: voltage security must be the top priority for the day, ahead of cost and ahead of service, because the end of the feeder has been running close to its limits. Service still sits in the middle. Please build the plan on this basis and do not come back to me with questions.

### r46 (Contradiction (should clarify), writer B, expected: clarify)

Handover note, day shift.

The operations manager confirmed this morning that, for the coming day, charging service is to be ranked above electricity cost: drivers should not wait long on a low battery and should leave with their requested energy, even if that means paying more.

The same manager also confirmed that bus voltages within limits remain the leading concern for the whole day.

In the afternoon call, the manager added that the electricity bill must take precedence over driver service tomorrow, since the budget review is still open. Nothing said in the morning was withdrawn, and both instructions are to stand as given.

No equipment faults to report.

### r47 (Multi-party ruling, writer B, expected: voltage > service > cost)

Minutes from this morning's dispatch call. The finance lead argued that tomorrow should be run to minimize energy purchase cost, since prices are expected to be high. The customer liaison wanted driver service put first, pointing to recent complaints about cars left nearly empty for hours. The protection engineer warned that the end of the feeder is running close to its lower voltage limit and asked that voltage be the top concern. After hearing everyone, the duty manager made the call, and that decision is what the dispatch will be evaluated against. Voltage security leads: keeping bus voltages within limits is the first priority. Service to drivers comes second, covering both waiting at low charge and leaving short of the requested amount. Purchase cost comes third; finance's concern is noted, but it ranks below the other two.

### r48 (Negation or concession, writer C, expected: cost > service > voltage)

This is not a grid-security study, and voltage is not the first thing we're judging. Nor is the goal to give every driver the best possible session regardless of what it costs. The day's dispatch should be assessed above all on how much we spend buying electricity. Slightly longer waits, or the occasional shortfall against a requested charge, are an acceptable price for a cheaper day, though service still has to be weighed and it outranks the voltage question. Some movement of bus voltages toward their limits is tolerable too; voltage still counts, only less than service and cost. Put simply, don't let concern for voltage override the drivers, and don't let concern for the drivers override the cost of energy.

### r49 (Background + final directive, writer C, expected: cost > service > voltage)

For context: the utility renegotiated our supply contract last month, and finance has flagged that the depot's electricity bill is now the line item under the most scrutiny. The grid side has been quiet; the voltage profile on this feeder has had plenty of margin all season, and drivers have mostly been satisfied with the service. With that said, this is how I want each day of charging dispatch evaluated from now on. The purchase cost of electricity is the primary yardstick. Next in importance is charging service: vehicles shouldn't sit for long at a low state of charge, and they shouldn't leave without the energy they requested. Keeping bus voltages within limits remains part of the assessment, but it ranks behind both cost and service.

### r50 (Missing priority (should clarify), writer B, expected: clarify)

Guidance for the coming day. Our first priority is the people who use the hub. A vehicle that arrives nearly empty should not be left waiting for long, and every driver should leave with the charge they requested. That is what the day will be judged on before anything else. Beyond that, there are two further things in play: keeping bus voltages inside their limits on the feeder, and what we pay to buy electricity. Both of them matter and neither should be ignored. I am not going to rank them against each other here; just make sure the schedule takes them into account after the drivers are looked after.

### r51 (Standard wording, writer A, expected: cost > service > voltage)

Electricity purchase cost comes first, EV charging service second, and voltage security last.

### r52 (Background + final directive, writer C, expected: voltage > service > cost)

A bit of background first, then the actual instruction. Over the past few weeks the feeder has been running close to its limits on several occasions, mostly on the long laterals far from the substation, and the protection team has asked us to stop treating that as a minor nuisance. Meanwhile the charging hubs have been fairly busy, and the purchasing desk keeps reminding everyone that wholesale prices have been volatile. None of that background is the instruction itself, so here it is plainly. When you judge a full day of charging dispatch, the first thing to look at is whether bus voltages stayed inside their limits. After that, judge how well the drivers were served: vehicles should not be kept waiting long while their batteries are low, and each one should depart with the energy its driver requested. The electricity purchase bill comes last; it still counts, but only after the other two.

