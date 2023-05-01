Download Link: https://assignmentchef.com/product/solved-cis301-homework-2-working-with-system-requirements-in-propositional-logic
<br>



<strong>Purpose of Assignment: </strong>

<ul>

 <li>To help you understand how to translate natural language requirements into propositional logic</li>

 <li>To help you understand how to use a SAT/SMT solver to automatically reason about propositional logic, and in a very simple fashion illustrate a few of the benefits of SAT/SMT solvers</li>

</ul>

<strong>Background: </strong>

A Patient-Controlled Analgesia (PCA) pump is a medical device often used in clinical settings to intravenously infuse pain killers (e.g., opioids) at a programmed rate into a patient’s blood stream through an IV line.   KSU CS researchers in the SAnToS Laboratory and their partners at

Adventium Labs are funded by the Department of Homeland Security to develop an open source safe/secure implementation of a PCA pump.   For more background information on PCA Pumps (not required for this assignment), see our project website:

<u>http://openpcapump.santoslab.org/node/1</u>

In this assignment, you will translate some example requirements of a PCA pump into propositional logic.

<strong>Information for This Homework </strong>

There are three basic ways that a PCA pump can be commanded to infuse opioids to a patient:

<ol>

 <li>A clinician may program the pump to deliver a ​<em>basal infusion </em>​– a relatively low-level amount of opioid that is continuously infused into the patient</li>

 <li>If the clinician senses that the patient is in pain, the clinician may command the pump to give a burst of opioid (called a ​<em>clinician </em>​bolus or ​<em>square </em>​bolus dose)</li>

 <li>If the patient feels like they are in pain, they can press a button attached to the pump to receive a ​<em>patient </em>​bolus dose. This allows patients to have some control over their own pain relief.</li>

</ol>

The pump motor mechanism operates at different rates to infuse opioids as appropriate for the current mode.  For example, the pump may infuse at a slower rate for the basal infusion and then change to a higher rate when a clinician bolus or patient bolus dose is requested.

Over-infusion of opioids can lead to respiratory depression and brain damage or death.   Therefore, PCA pumps have a variety of safe guards built into them.   Here are some examples that are relevant for this homework:

<ul>

 <li>When the patient pushes the patient bolus button and patient bolus dose is delivered, pushing the button again will have no effect until a ​<em>lockout interval </em>(e.g., 10 minutes) has passed.</li>

 <li>There is a ​<em>max volume of drug </em>​per hour that can be infused. This max amount is configured by the clinician during the patient/pump set up.  Once that limit is reached, no more drug will be infused until the hour has passed.</li>

</ul>

The pump also has different types of sensors to automatically detect various types of problems with the pump that may affect its safety or effective operation.

<ul>

 <li>The PCA pump device uses a pressure sensor in the fluid lines to detect when there is an “upstream occlusion”, i.e., a blockage between the drug reservoir and pump mechanism that prevents opioid from flowing through the pump. Similar sensors are used to detect a “downstream occlusion”, i.e., a blockage between the pump mechanism and the patient (e.g., due to a kinked IV line).</li>

 <li>The PCA pump device uses a sensor to detect air bubbles (“air embolisms”) in the fluid line. Infusing air bubbles into a patient’s circulatory system can be harmful.</li>

</ul>

Below is a diagram of PCA pump operational modes developed by researchers at the University of Minnesota.  Each rounded-corner rectangle represents a mode.  Some modes contain sub-modes.  For example, ​<strong>Idle</strong>​ and ​<strong>Therapy</strong>​ are submodes of the <strong>On</strong>​ mode; ​<strong>Paused </strong>and ​ <strong>Active</strong>​        are submodes of the ​     <strong>Therapy</strong>​        mode; ​            <strong>Basal</strong>​ , ​ <strong>Square</strong>​         <strong> Bolus</strong>​ and ​<strong>Patient Bolus</strong>​ are submodes of the ​<strong>Active</strong>​ mode.

In the full requirements document, every sub mode is subject to all the requirements defined for its parent mode in addition to the specific requirements defined for the new mode.   An important class of requirements are those that define/constrain transitions between modes.

<strong>Questions: </strong>

The questions for this assignment are divided into two parts. In the first, you will be formalize natural language. In the second, you will use Z3, an SMT solver.

<strong>Part 1:</strong>

Following the approach outlined in the 301 lecture <em>English to Propositional Logic</em>​       ​, formalize each of the requirements listed below in propositional logic.  For each requirement, indicate your propositional variables, the intuitive meaning of the propositional variables, and then give the propositional formula that formalizes the requirement.

Example Requirement:

<em>The PCA Pump shall detect downstream occlusions and upstream occlusions.  </em>

Solution:

<ul>

 <li>Propositional Variables o P = “The PCA Pump shall detect downstream occlusions” o Q = “The PCA Pump shall detect upstream occlusions”</li>

 <li>Logic formalization of Requirement: P ^ Q</li>

</ul>

General Hints:

Propositional logic doesn’t enable us to talk about sequences in time, so capturing the exact meaning of something like “transitions to IDLE mode” is difficult.  In such a case, we might make a convention to talk about the “current mode” and the “next mode”.   That still doesn’t tell us about what we would allow as the maximum delay in getting to the next mode, but we will ignore that issue in this assignment.

<ul>

 <li>When talking about a current mode, e.g., “the device is IDLE mode”, define a propositional variable (e.g., P) to be P = “current mode is IDLE”</li>

 <li>When talking about transition to a new mode, e.g., “transitioning to PATIENT bolus mode”, define a propositional variable (e.g., P) to be P = “next mode is PATIENT”.</li>

</ul>

(1) (3 points) Requirement (regarding a safety feature that guards against the infusion of air bubbles).




<em>If an air bubble is detected, the device shall raise an alarm on the operator interface and transition to IDLE mode. </em><em> </em>

(2a) (2 points) Requirement:




<em>The device is in PATIENT BOLUS sub mode only when it is in ACTIVE mode </em>




(2b) (1 point) Based on the presentation of the mode diagram above, why would it be incorrect for the requirement to be phrased as “The device is in PATIENT BOLUS sub mode if and only if it is in ACTIVE mode.”?




<ul>

 <li>(3 points) Requirement:</li>

</ul>




<em>The device shall infuse at the basal rate F</em>​<em>basal</em>​<em> when the device is in BASAL submode and only for that sub mode.</em>

<ul>

 <li>(3 points) Requirement:</li>

</ul>




<em>If the device is in Active Mode, it is either in the BASAL sub mode, SQUARE BOLUS sub mode or PATIENT BOLUS sub mode. </em>




<ul>

 <li>(3 points) Requirement:</li>

</ul>




<em>When the patient bolus button is pressed and the device is in Active Mode and the lockout interval is not in effect, the device shall transition to </em>

<em>PATIENT BOLUS sub mode unless it is in SQUARE BOLUS sub mode.</em>




<ul>

 <li>(4 points total) Getting requirements into a canonical form – we saw in our lecture that there are many different English language works that can be mapped to each operator. For example, “but”, “however”, “moreover”, etc. all mapped to “and”. It almost always improves understanding of the requirements if we always write them consistently in terms of the same words (for example, here is a link to a consistent approach championed by an engineer from Intel called “EARS: The Easy Approach to Requirements Syntax”.</li>

</ul>




<u>https://www.iaria.org/conferences2013/filesICCGI13/ICCGI_2013_Tutorial_Terzakis.pdf</u>




For this problem, we will NOT use the Intel approach, but will adopt our own simple strategy:

<ul>

 <li>Replace all words that correspond to the logical operator ^ with “and”</li>

 <li>Replace all words that correspond to the logical operator v with “or”</li>

 <li>Replace all words that correspond to the logical operator -&gt; with “if … then…” with the “if” portion of the sentence occurring before the “then” portion of the sentence.</li>

 <li>Replace all words that correspond to the logical operator &lt;-&gt; with “if and only if”</li>

 <li>Replace all phrases “P1 or P2 or … or Pn” where the “or” corresponds to an exclusive or with “P1, P2, … or Pn, exclusively”</li>

</ul>




For 6a, 6b, 6c, and 6d, use this approach to rewrite requirements 1, 2, 3, and 4 respectively (making use of the logical representations you already created).




<strong>Part 2: </strong>




Problems in this part require solutions in Z3 and Logika. You have two ways to run Z3 scripts. You can either use <u>https://rise4fun.com/z3</u><u>​            </u> or the command line utility<u>​ </u> built into your Sireum installation. The program is located at

&lt;SIREUM_HOME&gt;/apps/z3/bin/z3. ​ Note that on Windows, the slashes are reversed. On​   departments machines, you can simply use z3​ .​ To use the program, type ​       &lt;​ program&gt; -smt2 &lt;input&gt;. Where​     &lt;​ program&gt; is the path to the program and​      &lt;​ input&gt;​ is the name of the Z3 file you want to run.




<ul>

 <li>(5 points total) Automatic reasoning tools like Z3 solvers allow us to debug our specifications by generating test cases. We will see that in a much more impressive much later in the course, but for now we can use Z3 in a direct/manual way following the four steps below explained in the Automated Reasoning for Propositional Logic  lecture:</li>

</ul>




<ol>

 <li>declare all symbols involved in the formula</li>

 <li>translate the formula and then assert it</li>

 <li>issue the check-sat command</li>

 <li>get model(s) (if applicable)</li>

</ol>




Consider the following requirement:




<em>When the device is in OFF mode, it is neither in PAUSED mode nor is the pump infusing a drug. </em><em> </em>

Consider a scenario where a company making the PCA Pump recently hired two employees Jill and Josie who were asked to review the requirements.   To make sure they understood the requirements, Jill and Josie tried to study how the requirements used English words like “unless”, etc. that didn’t have an extremely clear logical meaning.




Both Jill and Josie agreed that the requirement above had three main atomic propositions:




<ul>

 <li>= Current mode is OFF mode</li>

 <li>= Current mode is PAUSED</li>

 <li>= Pump is infusing a drug</li>

</ul>




Jill argued that a direct rendering of the formula in propositional logic would be:




P -&gt; ~(Q V R)




Josie argued that formula was equivalent to




P -&gt; (~Q ^ ~R)




…and that it would be better to reword the English requirements to always use “If” instead of “when” and to stick with using “and”, “or”, and “not” that always have a clear interpretation to Boolean logic instead of using words like “neither” and “nor” which don’t have a distinct symbol for them in Boolean logic.

Josie wanted to write…




<em>If the device is in OFF mode, then the device is not in PAUSED mode and it is not infusing a drug. </em>




Since Jill and Josie are new on the job, they want to make sure that the different ways of writing the requirement are equivalent.




Show how you would help Jill and Josie prove the two formula above are equivalent using Z3, i.e., show that (P -&gt; ~(Q V R)) &lt;-&gt; (P -&gt; (~Q ^ ~R)) is a tautology (remember that “=” can be used for “&lt;-&gt;” in Z3).   See slides 60-61 and the associated FYTD exercises on slide 62 in the Automated Reasoning for Propositional Logic lecture.  The solutions for the FYTD Exercises on slide 62 can be found here:

<u>https://drive.google.com/open?id=0B2v9RmdjXXbUb0tkTXJaSzJaNE0</u>




<ul>

 <li>(3 points) Write a Z3 file that proves the equivalence of the two formulas above. Your file must run to receive full points.</li>

</ul>




<ul>

 <li>(2 points) In a comment in your Z3 file, explain why the output of your file really does prove the equivalence of the two formulas above. In Z3, end-of-line comments start with a ;​</li>

</ul>




(8) (10 points total) SAT and SMT solvers are sometimes used to help generate test cases.




Consider the following requirement:




R1:  When the total volume of drug infused in the current hour reaches the <em>max</em>​<em> volume of drug </em>​per hour limit, the device transitions to PAUSED mode and a notice that the max volume of drug per hour limit has been reached is displayed on the operator interface.

<em> </em>

The requirement R1 can be expressed as a propositional logic formula that has the following three atomic propositions:




<ul>

 <li><em>= “</em>​ the total volume of drug infused in the current hour reaches the <em>max</em>​<em> volume of drug </em>​per hour limit<em>“</em>​</li>

 <li><em>= “next mode = PAUSED” </em></li>

 <li><em>= “max volume per hour limit has been reached is displayed on operator interface” </em></li>

</ul>

<em> </em>

Since the formula has three propositional variables, there will be 8 rows in its truth table (e.g., 8 test cases that will cover all possibilities of the system state relevant to the requirement above).




Following the “recipe” given in class, use Z3 to generate the rows in the truth table (i.e., “tests”) for the requirement R1.




See slides See slides 44-58 and the associated FYTD exercises on slide 59 in the

Automated Reasoning for Propositional Logic lecture.  The solutions for the FYTD Exercises on slide 59 can be found here:

<u>https://drive.google.com/file/d/0B2v9RmdjXXbUblZBTDZIQm1YcGM/view</u> Complete these steps:

<ul>

 <li>Express R1 as a propositional logic formula.</li>

 <li>Start a logika truth table using the expression.</li>

 <li>Write a Z3 file to find and output a satisfying case for the expression.</li>

 <li>Add this case to your truth table and complete the row.</li>

 <li>Follow the “recipe” illustrated in class to assert the negation of the last case and find and output another satisfying case.</li>

 <li>Repeat (d) and (e) until there are no more satisfying cases.</li>

 <li>complete your truth table</li>

 <li>Give a one sentence English summary of the intuition of each of the rows in the truth table.</li>

</ul>




To receive full points:

<ul>

 <li>The truth table must have the correct expression</li>

 <li>The truth table must verify</li>

 <li>The Z3 file must have the correct initial assertion</li>

 <li>Each additional assertion must match an already outputted case</li>

 <li>The Z3 file must run without errors</li>

</ul>