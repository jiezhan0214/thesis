# Notes for paper

Here are thoughts, plans, records, etc, regarding to this paper. 

# About comments

- "approach" Yes, "framework" and "model" I think should be separated, "framework" might be weak. 
 
- "optimal" -> "efficient"? Not sure 

- percentage 0-100% to 0-1? OK

- how phrase/construct contributions, how much info I should provide, and the last sentence in abs. Think

- In contributions, can I say EHIC system in the beginning like "In summary, the main contributions of this paper to EHIC systems are: " No

- "failure" to "interruption" for consistency? Yes

- journal target? Not decided

- a few keywords: enable exploration, provide recommendation

# To Do

## Should do

- forward progress as the ratio of the effective execution time to the total time, what is "total time"?

- Do Alex comments and change structure. (Done)

- Sleep power reduction with comparator. (Done) 

- Load current consumption vs Supply voltage (Done)

- Related work unclear, how is my work different from others? What have we done differently from those previous works? Write in the last paragraph of Section II. (~Done) 

- Argument on Jackson's paper is not sound, to be improved. (Done)

- Time-based (compute in time order) vs sort distribution (~Done, Attached in ModelSetup last paragraph)

- Capacitance tolerance effect on forward progress tolerance. (Done)

- Reliable reference for the sentence with the first footnote in the Intro 

- Talk a bit about the effect of sizing capacitor on all kinds of transient computing approaches as a whole. 

- cite Prze

- text about Fig. 9 (Done)

- why choose 43uF in experiment? (Done, suggested by sizing process)

- in Fig. 6, add absolute progress improvement? 

- Examples of "atomic operations" or "the most energy-expensive" operation, as Ed points out. (Done)

- Use SIunit package to make those numbers tidy. (Done)


## Might do/write

- Number of power cycles to approximate constant power. 
  See "About Constant Current Test"

- Indoor light dataset. (Done)

- I_sleep denotes sleep consumption and voltage monitoring consumption.
  An ambiguous symbol. Put a definition of I_sleep. (Done)

- Irradiance fallback MPP effect, causing a small increase in throughput 
  than the theory projects. 

- Talk about Save without Vcore (volatile state lost, restore every time) vs Save with Vcore?

- Use "Sensor node" or "Device" or "System"?

- Title, also intro and conclusions, should imply the future

- Citations, make sure you cite thoroughly, and cite the paper in the target journal. Look the target journal, how are those papers structured there?

- In literature, Energy storage in transient computing?

- A possible finding from Table VI: in energy-scarce conditions, sizing ES is better; in energy fluent conditions, sizing EH is better. How to analyse dimensional costs of both? (mentioned now)

- Talk about recharging time at the end of trade-off?

## Remember to write somewhere clearly in text

- Metrics of forward progress: on time ratio at first, and then the program completion speed in experiments. (done)

- Def. of minimum capacity (defined in intro)

- Def. of volatile state (defined in intro)

- Def. of power cycle (defined as operating cycle in the first half, remove this word, use operating cycles)

- Def. of operating cycles (kind of defined in intro and in model)

- Def. of I_sleep (defined in model)

- Minimum energy storage capacity in simulation setup. 

- We use a LPM where CPU and SRAM are still powered (volatile state remained), so we don't need to restore after power recovers without brownout. But if using a LPM that cuts off supply for CPU and SRAM, Restore has to be performed every time. 

- Leakage of capacitor: We measure the leakage of the capacitors we are using, and it's low so we omit this in validation. However, without limited to this specific case, we use a leakage current value provided by the manufacturer. 

- The model can be used to fast explore how large the energy harvester should be in order to achieve an expected performance. 

# About Constant Current Test

If sleep without state retained, volatile state lost after hibernation.
Restore on every time.
Curve starts at 0.

If sleep with state retained, volatile state remained after hibernation. 
No restore after hibernation if staying in sleep mode (not brown out).
Curve starts at a higher level. 


# Model Assumptions

- Constant current. If current income and consumption are not constant, instead varying quickly, the model equations still hold, the exploration results do not stand, but we can redo the simulation in time order and high-resolution time steps to obtain the corresponding results. 

- Long supply period such that the cost of first restore can be omitted. 


# Independent Variables (Dimensions of the exploration space)

There is a multi-dimension exploring space in this paper, 
which includes following variables.

- Energy storage capacity
    - Voltage thresholds gap
    - Capacitance / Energy storage capacity

- Supply current
    - Harvester (size, efficiency)
    - Energy source condition

- Load current
    - frequency
    - workload (e.g. cache hit ratio)
    - peripheral usage 

- Restore / Save time overhead. Alternatively, RAM usage/volatile state size.


# Basic Idea / Narrative (at least as I read the intro)

Not clearly consistent with the real work. 

Design specifications: forward progress vs start-up time vs dimensions

"The trade-off in sizing energy storage should be studied for making design decisions in actual deployment." Reducing overheads vs leakage.

"However, there is not a method for designers to seek a cost-efficient energy harvester size to meet their design specifications in real-world deployment."

Contributions:

1. A model framework and an accurate load model for rapid design space exploration.
2. Design exploration which shows that properly increased capacitance brings forward progress optimisation with possible overhead. 
3. Sizing process


# Problems

- Capacitor, using tantalum to represent sizes, but using leakage data of an aluminum capacitor?
  According to a [reference](https://www.avx.com/docs/techinfo/Low_Leakage_Current_Aspect_Designing_Tantalum_Niobium_Oxide_Capacitors.pdf), the max leakage equation of AVX TAJ capacitor is given as 0.01CV. Redo those tests with 0.01CV?
  
  How should I verify the model without a reasonably accurate capacitor leakage model?

  Also, in my story, we identify the optimal capacitor size by assuming a leakage effect. And the capacitor is the only reason why there is an optimal capacitor size in the model. But in the experiment, we assume no leakage. 

- In addition to the last point, capacitor leakage is quite low in the last practical experiment. As remembered in experiment, leakage below CPU operating threshold seems to have two stages: 
    + 1st stage: around 1.8V - 1.0V, probably when SVS is on. ~9uA
    + 2nd stage: below 1.0V, leak slower than 1st, and looks like the "true" capacitor leakage.

- I am doing a thing which is of a very restricted scope. How can I argue my paper under such a 'scope' problem?
  + Energy harvesting devices are able to do more stuff than computing now, 
  which is more power-hungry and of a distributed power consumption range. 
  + Also, intermittent computing includes many other methods. Ideally, you should 
  do a model for general cases. 
  + How will variable current, instead of constant current (as assumed in the paper) affect the performance of the model and the results?

- Other issues in sizing capacitor
    + peripheral operations (large tasks)
    

- How does the model take Vsupply as an input and output Iload?


# Some messages that I think I don't communicate well in the paper

- Sizing energy storage is crucial for forward progress, but prior papers in transient systems usually ignore it, i.e. they minimise it for simple reasons or they ignore it.

- Leakage is not the only downside that drag forward progress down when increasing capacitance. Using a larger capacitor also prevents strong but short-term power pulses from activating the MCU. The restore threshold is hard to reach, so the MCU rarely starts processing. In this case the energy is wasted below the restore threshold. 

# Old Doc

Paper Abstract and Structure

New Abstract:
Forward progress in battery-less devices powered by intermittent energy harvesting sources is maintained by reactive intermittent computing, where volatile computing state is saved into and restored from non-volatile memory before and after power outages. To reduce device dimensions and improve responsiveness, energy storage, e.g. a capacitor, in such systems is minimised to a level which is just enough for the most energy-expensive atomic operation, e.g. saving and restoring state. In this paper, however, we show that minimising the storage may cause expensive state saving and restoring overheads; increasing the storage capacity instead leads to forward progress improvement, but compromises storage dimensions and responsiveness. To illustrate this trade-off, we formulate the relationship between forward progress and energy storage capacity given different power supply levels. Based on this relationship, we explore the design space of energy storage and energy harvester with respect to forward progress, responsiveness, and device dimensions. We further propose a method of sizing energy storage and energy harvester for energy-harvesting devices in real-world deployment. We validate our formulation via experiments based on an MSP430FR6989 microcontroller, where the results show that increasing storage capacity from xxuF to xxuF improves throughput rate by xx-xx% with xx-xx% reduction of state saving and restoring overheads, while the storage volume increase by 0-xx% for various off-the-shelf capacitors. The suggested energy storage capacity of our sizing method increases computation throughput by xx% and xx% for solar and RF energy sources respectively. 

Old Abstract: 
Forward execution in battery-less devices powered by intermittent energy harvesting sources is maintained by reactive intermittent computing, where volatile computing state is saved into and restored from non-volatile memory before and after power outages. With the goal of reducing dimensions, energy storage, e.g. a capacitor, in such systems is minimised to a level which is just enough for the most energy-expensive atomic operation, e.g. saving and restoring state. In this paper, however, we show that minimising the storage may increase the frequency of power outages, and hence, expensive state saving and restoring overheads; increasing the storage capacity instead leads to application throughput improvement, but compromises storage dimensions. To illustrate this trade-off, we formulate the relationship between the energy storage capacity and the execution throughput rate given different power supply levels. Based on this formulation, we propose a method of sizing energy storage for energy-harvesting devices in real-world deployment. We validate our formulation via experiments based on an MSP430FR6989 microcontroller, where the results show that increasing storage capacity from xxuF to xxuF improves throughput rate by xx-xx% with xx-xx% reduction of state saving and restoring overheads, while the storage volume increase by 0-xx% for various off-the-shelf capacitors. The suggested energy storage capacity of our sizing method increases computation throughput by xx% and xx% for solar and RF energy sources respectively.

Power outages?
As pointed out in the viva, a “power outage” denotes voltage supply dropping below the minimum operating voltage.

Atomicity: just a minimum energy storage

Response time/Sensitivity/Start-up energy/Wake-up requirement: this is about charging from below min. operating voltage. The time/energy to charge is relevant to the voltage before charging, the sleep&detect power, wakeup threshold, storage capacity. A maximum charging time can be calculated by assuming charging from 0V. Start-up energy can increase, some small energy bursts that should have waken up, now might be just turned into an energy increase.

I think a confusing/annoying thing of my work is that it improves computation throughput, but a typical application includes peripheral operation, which consumes energy like “burst” (large and short) rather than “trickle” (small and constant) as computation. If I only talk about computation, I need to justify that in what application it is important to compute fast. 

I think I should talk about this in a more practical scenario to make this paper meaningful. SO, application with peripheral? If use peripheral, the result will be further undermined. 

Using a large storage, prolong power cycle, to gain more progress is quite obvious.


Structure:

Introduction
	Similar to the abstract.
	Reactive, static, performance issue, related works.

Energy Storage in Reactive Intermittent Computing (Background and Motivation)
Explain the principle of reactive intermittent computing (perhaps move this to introduction)
Show the performance change, start-up/response time change given a minimum storage and an increased storage, with a few example experiment results of computation throughput and operating voltage traces. 
To explore the storage capacity in a wider range and a better granularity than just a few examples, briefly introduce the time-traversal model and present the simulated results. Probably point out the model accuracy/error by comparing the simulated results with the experimental results before.

Theory and Formulation of Capacity-Performance Relationship
Present the reasoning of this formulation, and its presumptions.

Energy Storage Sizing Method for Reactive Intermittent Computing Devices in Real-World Deployment
Present this sizing method. Basically, it puts the expected energy conditions into the formulation, generates the estimated performance distribution, and accumulate this distribution to get the overall throughput. 

Experimental Validation
Experimental setup: platform, application/benchmarks.
Validate the formulation with an experiment of the capacity-performance relationship.
Show the compromise of start-up time.
Show the compromise of actual storage dimensions with several off-the-shelf capacitor datasheets.

Case Studies of Sizing Method
Solar
RF
Solar & RF in combine?

=========================

Questions:
what about the time-traversal model? Do I need to fit this work in? This can be fit in 1&2 below. In 1, show the experimental examples, briefly introduce the model, then present the simulated results in a wide spectrum (and compare to the experimental results to show the accuracy of this model). In 2, after proposing the formulation, compare the formulation to model and experiment data?.
How to encompass the changes in supply? Does this formulation/sizing method work for a volatile supply?
Case study of sizing method: if we simulated the solar energy for a year, this would be a simulated result that we won’t be able to validate.
Is it really power outage I am saying? Or the triggered saving operations/interrupts, to be specific. 
In the abstract, is reducing dimensions the only goal of minimising storage? What about minimising response time, increasing active time (and hence more computation)?
