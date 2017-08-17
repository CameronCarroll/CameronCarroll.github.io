---
title: Aztec Electric Racing -- First Semester Review (Manufacturing Phase 1)
category: engineering
layout: back
month_year: Summer 2017
---

<b><a href="/engineering/2017/07/17/AER-Year1-Design.html">Link to Part 1: AER's first semester of design</a></b>

Overview of Spring Design/Manufacturing Phase
------------------------
According to the schedule, designs were to be frozen in December. In reality, design work continued up to the days of competition and was never finished. In our second semester, while the rest of the team was quite successful in building the chassis, driveline, and mechanical systems of the vehicle, Tractive/Controls struggled to research, design, purchase for and build the electrical backbone to the vehicle.
<b><a href="/engineering/2017/07/17/AER-Year1-Design.html">Link to Part 1: AER's first semester of design</a></b>

* **January and February**
    * Electronic component selection begins for Tractive system; Controls already purchased some sensors and switches, but we lacked connectors and components for the high voltage system.
        * Downsides to choosing maximum allowed power become apparent, as connectors and components rated for 300-400A are expensive and scarce. Team understood generally that components can be rated for continuous expected current rather than peak current, but did not have the knowledge or data to support such a decision.
    * Worked on design and components for our first "cheap" accumulator (a relative term -- Lithium cells are *expensive!*) up through February, before scrapping and starting over with our LG Chem cell design.
    * The team struggled for a long time (months) with the weight issue, with an original target weight of 475 lb without driver. (At competition, our car weighed over 700 lb.) A team member working at an EV shop was able to get us many thousands of dollars worth of components, but by using them we completely blew our weight budget.
        * By mid-February, the team management and advisor were on board with the super high weight design because it was the obvious only choice available. The time lost waiting for this final approval frustrated the Tractive system, but I don't think it ultimately made a difference in whether or not we competed, due to deficiencies in the safety and Controls systems.
* Mechanical parts of Tractive system begin to come together:
    * Zytek 451 drive assembly separated into components.
    * Battery cells finalized, but all other details were worked out on the fly during this time.
    * Up until a month or so before competition, we did not have a solution for motor controller, as the Zytek unit uses proprietary codes which have not been reverse engineered for general use. Finally, we got in touch with Sevcon, who tuned their latest and largest model to drive the Zytek motor. (Gen4 Size8 for $3000ish)

* **March througuh May**
     * Our Controls lead graduated at the end of the first semester, leaving essentially nothing in terms of institutional knowledge. The replacement, although enthusiastic, was overwhelmed and lacked the circuit design knowledge required. This author ended up adopting the circuits and safety system during the second semester.
        * This seems very self-centered, but it truly felt like these documents and systems moved forward only as quickly as I could understand them. It was a totally new area of knowledge for everyone on the team; One team member working in an EV shop had many years of practical design experience, bringing immeasurable knowledge and resources to our team, but often approached problems with less rigor/analysis than this author agreed with. Aside from him, team was very experienced with all of the mechanical apects of the car, but electrical knowledge was rather limited. (People worked on what they knew because there was so much to finish.)
        * By March, I decided to adopt the ESF and FMEA documents. The first step was to gut them of the bullshit, a task more easily done by starting over with a blank document.
            * Some of the team is still salty about these docs, having seen them as a waste of time. Personally, I saw them as great tools to guide our designs when we really had no idea about how to build a safe high-voltage system.
            * Again, some teammates might scoff at the idea that this is a 'safe' system as specified by the rules and the judges, but we're hardly at the level of engineering needed to implement these specifications, let alone to write the specifications ourselves.
    * As May turned to June, the electrical team finally began to understand the constaints to the safety circuit design, forcing us to quickly rethink the architecture:
        * Our first approach was 'naive,' with all functions distributed onto separate circuits. I don't believe this approach ever reached design-for-manufacturing and could have been consolidated onto a PCB if finished.
        * However, in an attempt to simplify the construction, our controls team redesigned the system to use CPLDs (or a single CPLD) with all of the sensor and switch circuits programmed in. A week or two was spent on this effort until...
        * We realized that the circuits must be implemented in non programmable logic, ie hardwired circuits. My understanding is that this is to prevent programming or firmware errors from invalidating an otherwise safe design.
        * Eventually our electrical engineering consultant put together a last-minute PCB containing the primary safety system circuits: BMS/IMD/HVD interlock relay control, brake pedal plausibility, and acceleration pedal handling.
            * Our team did not have the time/ability to put together these circuits on short notice, so this PCB served as a baseline allowing us to develop the rest of the safety system.

* **June**
    * By the month of our competition, the team had scrambled to pull the car together mechanically for a rolling chassis, and the Tractive system finished an accumulator design and mounted the motor and driveline.
    * The Controls system was in dire straits: The PCB design was outsourced to our consultant as a last-ditch effort to compete, other circuits were being designed and manufactured in the days just before competition, and critical issues in the design didn't become apparent until we arrived at Lincoln. (Consultant misunderstood galvanic isolation in the context of HV automotive systems, taking the marine electronics view instead.)
    * Components were ordered online too late to arrive in time for competition, forcing us to find quick replacements from local electronics/hardware stores.
    * The days before and of competition included furious electrical assembly, as the accumulator came together and the vehicle sensors and switches were wired up.
        * With 75 cells on which to add thermistor and voltage sense leads, it took Order(8) hours to wire and assemble the accumulator.
        * Room for packaging suffered due to the low-budget high-power design. Without proper time to plan wire routing and looming, the vehicle quickly became a mess of copper.
    * Deficiencies in Tractive designs:
        * Need to know actual outputs from equipment
        * Need to model losses as well as needs vs capability
        * No mystery components allowed -- Everything must have a datasheet. (An attempt at running a UL94-V0 rating test on a piece of insulation was rejected WRT edge cases of molten metal rather than a bunsen burner flame.)
        * (Sparing the long list of things that are missing.)
    * Deficiencies on Controls designs:
        * No ownership of the central PCB
        * System has never booted up
        * We'll find out once we try to boot the systems up and bench test everything. (Power supply acquired!)
