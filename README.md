Download Link: https://assignmentchef.com/product/solved-eel3744c-lab-2-i-o-timing
<br>
<h1>OBJECTIVES</h1>

<ul>

 <li>Understand fundamental input and output (I/O) concepts, in regard to microprocessors and microcontrollers.</li>

 <li>Learn how to manage timing with a software delay as well as with a hardware timer/counter (TC) system.</li>

 <li>Combine I/O and timing concepts to design an LED animation creator program.</li>

</ul>

<h1>INTRODUCTION</h1>

It is often desired that a microprocessor be able to interface with an external entity in order to either receive or transmit data. More generally, all computer systems are designed to utilize some form of <strong>input and output </strong>(<strong>I/O</strong>). Without these, there would simply be no practical purpose for a computer system.<a href="#_ftn1" name="_ftnref1"><strong><sup>[1]</sup></strong></a>

Separately, another important concept in the world of microprocessors is timing. Normally, computer applications require that some thread of execution occur at a specific point in runtime; as a basic example, to blink an LED, a low voltage must be applied to the LED during some intervals(s) of time, and a high voltage must be applied at all other times. In applications such as these, a microprocessor can sometimes manage timing by simply performing some set of meaningless instructions on purpose.<a href="#_ftn2" name="_ftnref2"><strong><sup>[2]</sup></strong></a> In this context, these meaningless instructions constitute what is known as a <strong>software delay</strong>. Oftentimes, a more accurate and advanced approach of utilizing a <strong>hardware timer</strong>, or an independent electronic system that keeps track of time, is chosen. Within the <em>ATxmega128A1U</em>, programmable hardware timers exist within the <strong>R</strong>eal-<strong>T</strong>ime <strong>C</strong>lock (<strong>RTC</strong>) and <strong>T</strong>imer/<strong>C</strong>ounter (<strong>TC</strong>) systems.

Overall, understanding how to utilize both I/O and timing is crucial for being able to create any meaningful program for a computer system.

<h1>LAB STRUCTURE</h1>

In § 1 of this lab, you will explore basic input and output (I/O) concepts, as well as learn to interface your microcontroller with two basic

I/O components available on the <em>OOTB Switch &amp; LED Backpack</em>: DIP switches and LEDs. Next, in § 2 and § 3 of this lab, you will begin to utilize both of the aforementioned timing mechanisms, that is, both software and hardware timers. Lastly, in § 4, you will design a program that creates LED animations; this will utilize a switch on the <em>OOTB Memory Base</em>. This comprehensive application will utilize I/O components as well as timing mechanisms to allow you to create, edit, and display any “8-bit animation” of up to almost 7 minutes in length.<a href="#_ftn3" name="_ftnref3"><strong><sup>[3]</sup></strong></a>




<h1>REQUIRED MATERIALS                                  SUPPLEMENTAL MATERIALS</h1>

<ul>

 <li><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8331_XMEGA_AU_Manual.pdf">Atmel XMEGA AU Manual (doc8331)</a> <a href="https://mil.ufl.edu/3744/labs/multiple_asm_files.gif">Adding Additional Program Files To An Atmel Studio</a></li>

 <li><a href="https://mil.ufl.edu/3744/docs/uPAD2p0/schematics/3744_switchLED_backpack%20v1.3.pdf"><em>OOTB Switch &amp; LED Backpack Schematic</em></a> <a href="https://mil.ufl.edu/3744/labs/multiple_asm_files.gif">Project (GIF)</a></li>

 <li><em>OOTB µPAD v2.0</em> with USB A/B cable <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf">Atmel </a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf"><em>ATxmega128A1U</em></a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf"> Manual (doc8385)</a></li>

 <li><em>OOTB Switch &amp; LED Backpack </em>           <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc0856_AVR_Instruction_Set.pdf">AVR Instruction Set (doc0856)</a></li>

 <li><em>OOTB Memory Base</em>           <a href="https://mil.ufl.edu/3701/DAD/Waveforms_2015_tutorial.pdf">UF </a><a href="https://mil.ufl.edu/3701/DAD/Waveforms_2015_tutorial.pdf"><em>WaveForms</em></a><a href="https://mil.ufl.edu/3701/DAD/Waveforms_2015_tutorial.pdf"> Tutorial</a></li>

 <li><strong>D</strong>igilent <strong>A</strong>nalog <strong>D</strong>iscovery (<strong>DAD</strong>) with <em>WaveForms</em>           <a href="https://mil.ufl.edu/3701/DAD/DAD-vids.html">Digilent </a><a href="https://mil.ufl.edu/3701/DAD/DAD-vids.html"><em>WaveForms</em></a><a href="https://mil.ufl.edu/3701/DAD/DAD-vids.html"> Video Tutorials</a></li>

</ul>

software                                                                                                           •    <a href="http://ww1.microchip.com/downloads/en/AppNotes/doc8045.pdf"><em>Using the XMEGA Timer/Counter </em></a><a href="http://ww1.microchip.com/downloads/en/AppNotes/doc8045.pdf">(doc8045)</a>

<ul>

 <li><a href="https://mil.ufl.edu/3744/docs/switch_debouncing_through_software.pdf"><em>Switch Debouncing through Software</em></a> <a href="https://mil.ufl.edu/3744/labs/lab2_u20_4_skeleton.asm">lab2_u20_4_skeleton.asm</a></li>

 <li><a href="https://mil.ufl.edu/3744/docs/tc_note.pdf"><em>The Most Common Use Case for Timer/Counters</em></a></li>

</ul>




<h1>PRE-LAB PROCEDURE</h1>

<strong><u>REMINDER OF LAB POLICY</u></strong>

You must re-read the <a href="https://mil.ufl.edu/3744/admin/Lab%20Rules%20%26%20Policies.pdf"><em>Lab Rules &amp; Policies</em></a> before submitting any pre-lab assignment and before attending any lab.

<h2>1. INTRODUCTION TO I/O</h2>




Most microcontrollers use <em>I/O ports</em> as a construct to share information with other devices. An <em>I/O port</em> is simply a collection of pins which can be individually configured for various purposes. Often, an <em>I/O port</em> is a conglomerate interface for both an <em>input port</em>, a collection of input signals, and an <em>output port</em>, a collection of output signals. These pins are referred to as General Purpose Input/Output (GPIO). GPIO ports on the <em>ATxmega128A1U</em> are configured and accessed via registers, which are mapped to addresses in data memory space. In most cases, these memory-mapped registers can simply be modified by any instruction which accesses data memory. Sometimes, as is the case in the <em>ATxmega128A1U</em>, an individual signal made available by an I/O port has the capability of serving as either an input or an output.

Within the <em>ATxmega128A1U</em>, there exist many I/O ports, all of which (collectively, or individually) allow the microcontroller to connect to the outside world. To grant even additional flexibility, each I/O port supports multiple configurations.

<strong>NOTES:  </strong>

<ul>

 <li>From the perspective of the <strong>c</strong>entral <strong>p</strong>rocessing <strong>u</strong>nit (<strong>CPU</strong>) within the <em>ATxmega128A1U</em>, the collection of all I/O ports constitute what is known as a <em>peripheral system </em>(a <em>system</em> <em>peripheral</em> <em>to </em>the processing unit), and each entity within the system is referred to as a <em>module</em>. Throughout this course, a wide variety of other peripheral systems, both inside and outside the microcontroller, will be utilized.</li>

 <li>Most systems within the <em>ATxmega128A1U</em>, including the I/O port system, are made to support dynamic configuration. To support dynamic configuration, data storage devices known as <em>registers</em> (flip-flop-like components) are utilized. Within the <em>ATxmega128A1U</em>, all registers that store configuration information, where these are otherwise known as <em>configuration registers</em>, are accessible through the use of dedicated memory locations (within the <em>“</em>I/O memory space”) and some appropriate memory-accessing instructions. When configuration memory such as a register, or anything similar, is designed to be accessible through such means, it is said to be <em>memory-mapped</em>.</li>

 <li>In the context of the <em>ATxmega128A1U</em>, most of the physical pins on the chip package directly connect to I/O port signals. Because of this, most pins on the chip package are named in terms of the I/O port signals! Additionally, instead of having separate dedicated signals for specific peripheral systems within the microcontroller, i.e., those that are not “<strong>g</strong>eneral-<strong>p</strong>urpose <strong>i</strong>nput/<strong>o</strong>utput (<strong>GPIO</strong>)” signals, most signals are multiplexed with those that connect I/O ports. Become familiar with § 33.2 <em>(Alternate Pin Functions)</em> within the 8385 manual, which lists all signals that are multiplexed with those that connect to I/O ports.</li>

 <li>If the provided lab kit was assembled correctly, when “backpacks” and “base boards” are plugged into the <em>OOTB µPAD</em>, some components, e.g., switches and LEDs, will be directly connected to some I/O port pins on the chip package.</li>

</ul>

In this part of the lab, you will learn how to interface your microcontroller with the DIP switches and LEDs available on your <em>OOTB <strong>S</strong>witch &amp; <strong>L</strong>ED <strong>B</strong>ackpack </em>(<strong><em>SLB</em></strong>), where these components represent input and output sources, respectively.

<ul>

 <li>First, understand how to utilize the memory-mapped I/O configuration registers by reading § 13 (<em>I/O Ports</em>) of the 8331 manual</li>

 <li>Study the <em>OOTB Switch &amp; LED Backpack </em>and its relevant schematic. Identify any necessary components, as well as where they connect to the <em>ATxmega128A1U</em>.</li>

</ul>

<h3>PRE-LAB EXERCISES</h3>

<ol>

 <li>Which configuration register allows the utilization of an I/O port pin configured as an input? Which configuration registers allow the utilization of an I/O port pin configured as an output?</li>

 <li>What is the purpose of the SET/CLR/TGL variants of the DIR and OUT registers?</li>

</ol>

<ul>

 <li>Which I/O ports are utilized for the DIP switches and LEDs on the <em>OOTB Switch &amp; LED Backpack</em>?</li>

</ul>

<ol>

 <li>Are the LEDs on the <em>OOTB Switch &amp; LED Backpack </em>active-high, or active-low? Draw a schematic diagram for a single LED circuit with the same activation level used on the backpack, as well as one with the opposite activation level. Also, draw a schematic diagram for a <strong>s</strong>ingle-<strong>p</strong>ole, <strong>s</strong>ingle-<strong>t</strong>hrow (SPST) switch circuit, using the same pull-up or pull-down resistor condition utilized on the backpack, as well as another switch circuit using the opposite configuration.</li>

 <li>Would it be possible to interface the <em>OOTB µPAD</em> with an external input device consisting of 24 inputs? If so, describe how many I/O ports would be necessary. If not, explain why.</li>

</ol>

To mount the <em>OOTB Switch and LED Backpack</em> (as well as any of the other backpacks) onto the <em>µPAD</em>, you must match the J1, J2, J3, and J4 headers with the respective ports. The cutout at the top of the backpack (as well as all other backpacks) should allow the reset button on the <em>µPAD</em> (labeled S1) to remain visible. See Figure 1 for an image of a correctly mounted <em>OOTB Switch &amp; LED Backpack</em>.

1.3. Connect the <em>OOTB Switch &amp; LED Backpack </em>to the <em>µPAD</em>.

<table width="735">

 <tbody>

  <tr>

   <td width="705">You will now create an assembly program (<strong>lab2_1.asm</strong>) to continually, i.e., within an endless loop, output the value of each DIP switch circuit located on the <em>OOTB Switch &amp; LED Backpack </em>to a corresponding LED circuit also located on the backpack.  More specifically, if an input switch is determined to be closed, the LED located directly below the switch on the backpack must be powered on (i.e., illuminated); conversely, if an input switch is determined to be open, the LED located directly below it must be powered off.1.4. Write an assembly program (<strong>lab2_1.asm</strong>) to allow the <em>µPAD</em> to emulate the behavior described above. <strong>Figure 1:</strong> <em>OOTB Switch &amp; LED </em>Backpack correctly mounted onto µ<em>PAD</em><strong>2.</strong> <strong>SOFTWARE DELAYS</strong></td>

  </tr>

 </tbody>

</table>

In this section, you will begin to utilize <em>software delays</em>. Software delays are a form of timing delays that are constructed with some (ideally small) set of instructions. Although software delays generally have the potential to be very precise, they prevent a microprocessor from executing other instructions, and ultimately cause CPU time to be wasted. Software delays are also extremely non-modular, as they cannot easily be used at other processor clock speeds, and if designed in an assembly language, cannot be directly ported to most other processors.

In general, a hardware timer/counter is preferable to a software delay, however software delays may sometimes be appropriate when a simple delay is needed. You will learn about the hardware timer/counters available within the <em>ATxmega128A1U</em> in the following section of this lab document.

To create a software delay, as described above, a (condensed, ideally) series of meaningless instructions should be performed by your microcontroller. The delay length created, in units of time, will be determined by the amount of instructions executed. (Depending on the series of instructions, the architecture within the microcontroller may add unintended execution time.)

Below, you will create an assembly subroutine to delay 10 ms via a software delay. When determining how many instructions your microcontroller should execute, it would be fair to assume that each one-cycle assembly instruction takes about 0.5 µs, since the <em>ATxmega128A1U</em> system clock runs at 2 MHz by default (1/[2 MHz] = 0.5 µs). Remember that frequency (f) is the reciprocal of period (T), i.e., f = 1/T. (Later in this semester, you will learn how to change the system clock frequency.)

2.1. Read through the pertinent sections of our course notes, § 3 (<em>AVR CPU</em>) within the 8331 manual, and the 0856 (<em>AVR Instruction Set</em>) manual to learn about how to write assembly subroutines as well as how to effectively manipulate “stack memory”.

2.2. Create an assembly file (<strong>lab2_2.asm</strong>). (See the <em>Supplemental Materials </em>section of this document for a GIF file that depicts how to add an additional assembly file and set it as the <em>entry file</em>, so that you can utilize multiple programs within a single assembly project of <em>Atmel Studio</em>.) Within this newly created file, first explicitly configure the “stack pointer” to have an initial value of the highest possible data memory address, as to allow the stack to be of maximal size. (Although the processor automatically performs this procedure, you are expected to perform it explicitly within all programs created throughout this course.) Following this, write a subroutine, DELAY_10MS, to simply delay ten milliseconds (i.e., 10 ms), without the use of an additional subroutine, e.g., DELAY_1MS. An error allowance of 3% is given. Finally, with the intention to test your delay subroutine, write a main routine (within the same assembly file) to toggle every 10 ms the output of an I/O port pin that is available for probing. To determine which I/O port pins are available for probing, review the appropriate <em>OOTB </em>schematics.

<strong>NOTE:</strong> Within a subroutine, when appropriate, remember to push/pop registers.

2.3. Program your microcontroller with the above assembly file; use your DAD, along with the <em>Scope </em>feature of <em>WaveForms</em>, to measure the rate at which the specified output pin toggles. (See the <a href="https://mil.ufl.edu/3701/DAD/Waveforms_2015_tutorial.pdf"><em>WaveForms</em></a><a href="https://mil.ufl.edu/3701/DAD/Waveforms_2015_tutorial.pdf"> Tutorial,</a><a href="https://mil.ufl.edu/3701/DAD/waveforms-2015_rm_23Feb17.pdf"> Digilent</a> <a href="https://mil.ufl.edu/3701/DAD/waveforms-2015_rm_23Feb17.pdf"><em>WaveForms</em></a> <a href="https://mil.ufl.edu/3701/DAD/waveforms-2015_rm_23Feb17.pdf">Reference Manual PDF</a><a href="https://mil.ufl.edu/3701/DAD/waveforms-2015_rm_23Feb17.pdf">,</a> and/or <a href="https://reference.digilentinc.com/reference/software/waveforms/waveforms-3/reference-manual">Digilent</a> <a href="https://reference.digilentinc.com/reference/software/waveforms/waveforms-3/reference-manual"><em>WaveForms</em></a> <a href="https://reference.digilentinc.com/reference/software/waveforms/waveforms-3/reference-manual">Reference Manual Website</a><a href="https://reference.digilentinc.com/reference/software/waveforms/waveforms-3/reference-manual">,</a> if necessary. Video tutorials are also available <a href="https://mil.ufl.edu/3701/DAD/DAD-vids.html">here</a><a href="https://mil.ufl.edu/3701/DAD/DAD-vids.html">.</a>) Make note of the delay time <strong>initially</strong> measured and include this in the Appendix of your pre-lab report.

2.4. Until the required accuracy is met, fine-tune your delay subroutine. Make notes of any additional delay times measured, including those with error greater than 3%, and include them in the Appendix of your pre-lab report.

Now,      you       will      create      an      additional       subroutine,

<table width="738">

 <tbody>

  <tr>

   <td width="733">delay of 2.55 seconds (255×10 ms), as well as a minimum delay of approximately 0 ms, be possible with your delay subroutine.2.5. Create            another subroutine, DELAY_X_10MS,        as described above.2.6. Alter your main routine to <strong>toggle</strong> an output pin at a rate of 20 Hz, i.e., generate a square waveform with a frequency of 10 Hz. An error allowance of 3% is again given.The routine described above should generate a square waveform

    <table width="733">

     <tbody>

      <tr>

       <td width="418">figure represents the period of the waveform.2.7. Measure the chosen output pin (again with the <em>Scope </em>feature of <em>WaveForms</em>) and verify that the <strong>toggle frequency</strong> is 20 Hz, i.e., that the overall waveform has a</td>

       <td width="315">is within the given error allowance, take a screenshot, including a frequency measurement of the waveform, and include this image in the Appendix of your pre-lab report.</td>

      </tr>

     </tbody>

    </table>with a 50% duty cycle, as shown in Figure 2, where 2X in the                               <sup>frequency of 10 Hz. When the frequency of the waveform </sup><strong>3.</strong> <strong>INTRODUCTION TO TIMER/COUNTERS</strong></td>

  </tr>

 </tbody>

</table>

DELAY_X_10MS, to delay a select multiple of 10 ms specified by the value of a general-purpose register passed into the subroutine. To do so, before the subroutine is called within a program, a specific register must be loaded with a value representing the number of 10 ms delays that should occur. (In essence, this register is storing a parameter, or an argument, of the subroutine.) Ultimately, to perform the necessary delay, the DELAY_X_10MS subroutine should call the DELAY_10MS subroutine however many times is specified by the register chosen to store the parameter. It is expected that a maximum In this section, you will learn the fundamentals of timer/counter (TC) systems within XMEGA microcontrollers. In general, timer/counters are far more useful than software delays, though normally are slightly more complex to utilize.

3.1. Read § 14 (<em>TC0/1 – 16-bit Timer/Counter Type 0 and 1</em>) of the 8331 manual. (The timer/counter systems explained in this section of the manual are those that you are required to use in this lab.) Then, read <a href="https://mil.ufl.edu/3744/docs/tc_note.pdf"><em>The Most Common Use Case</em></a> <a href="https://mil.ufl.edu/3744/docs/tc_note.pdf"><em>for Timer/Counters</em></a><a href="https://mil.ufl.edu/3744/docs/tc_note.pdf">.</a> For further supplemental information following these documents, you may also refer to doc8045 <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8045_TC.pdf">(</a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8045_TC.pdf"><em>Using the XMEGA Timer/Counter</em></a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8045_TC.pdf">)</a>.

<strong>NOTE:</strong> It is possible to split a TC0 system into two 8-bit timer/counters, and it is also possible to concatenate two 16-bit timer/counters into a single 32-bit timer/counter; however, neither of these need to be utilized for this lab.

<h3>PRE-LAB EXERCISES</h3>

<ol>

 <li>Assuming a system clock frequency of 2 MHz, a prescaler value of 256, and a desired period of 50 ms, calculate a theoretically-corresponding timer/counter period value two separate times: once using a form of dimensional analysis, providing explanation(s) when appropriate, and another time using the general formula provided within <a href="https://mil.ufl.edu/3744/docs/tc_note.pdf"><em>The Most</em></a> <a href="https://mil.ufl.edu/3744/docs/tc_note.pdf"><em>Common Use Case for Timer/Counters</em></a><a href="https://mil.ufl.edu/3744/docs/tc_note.pdf">.</a></li>

</ol>

<ul>

 <li>Assuming a system clock frequency of 2 MHz, is a period of two seconds achievable when using a 16-bit timer/counter prescaler value of one? If not, determine if there exists any prescaler value that allows for this period under the assumed circumstances, and if there does, list such a value.</li>

 <li>What is the maximum time value (to the nearest millisecond) representable by a timer/counter, if the relevant system clock frequency is 2 MHz? What about for a system clock frequency of 32.768 kHz?</li>

</ul>

3.2. Referring to <em>Pre-lab Exercise vi.</em>, write an assembly program (<strong>lab2_3.asm</strong>) to toggle an I/O port pin available for probing every 50 ms, utilizing a timer/counter where appropriate. After writing a complete initial version of this program, use the DAD and the <em>Scope </em>feature of the <em>WaveForms</em> software to experimentally determine which whole-number digital period value provides a corresponding period with the least amount of error. (Recognize that this whole-number may not be either the truncated or “rounded-up” version of the theoretically-corresponding value.) Provide an appropriate screenshot of the relevant waveform with the minimal amount of error, including its <em>precise</em> frequency. (Refer to the <em>Lab Rules &amp; Policies</em> for how to appropriately take a screenshot of a waveform as well as for how to appropriately measure a precise frequency.) Additionally, provide within the caption of the relevant screenshot the whole-number value that resulted in a minimal amount of error.

<strong>NOTES:  </strong>

<ul>

 <li>When writing to 16-bit registers (such as PER and CNT), the bytes must be written in order of least to most significant, otherwise the value will not be updated. This is due to the internal buffering used when writing to registers which represent values greater than 8-bits (§3.11 doc8331).</li>

 <li>In general, the native ATxmega128a1udef.inc file does not provide a direct symbol name for the address of a memorymapped register that holds a specific byte of some overall value greater than eight bits. For example, although the 16bit period value within timer/counter module TCC0 is represented with two separate memory-mapped registers, one dedicated to the “low byte” (PERL) and one dedicated to the “high byte” (PERH), there only exists the symbol name “TCC0_PER” which corresponds to the address of PERL. Consulting the offset TC register summary (§14.13 doc8331), PERH is mapped to the address immediately after PERL, and so corresponds to “TCC0_PER+1” (This pattern holds true for most, if not all, other register sets).</li>

 <li>The TC module provides an <strong>overflow flag</strong> (OVFIF in the INTFLAGS register) which is set in hardware whenever the overflow or underflow condition is met (depending on the mode of operation). Use of this flag is essential to the proper operation of the TC system. If the CNT value is compared manually, there is no guarantee that the overflow condition will be caught, as reading and comparing a 16-bit value requires multiple cycles to complete. In this lab the overflow flag will be polled synchronously and must be cleared when an overflow occurs in order to detect the <em>next</em> overflow event.</li>

</ul>

<table width="737">

 <tbody>

  <tr>

   <td width="242">

    <table width="737">

     <tbody>

      <tr>

       <td width="348"><strong><u>PRE-LAB EXERCISES</u></strong>ix. Create an assembly program to perform the same procedure as in § 3.2 but utilize a prescaler value of two. Perform everything else described in the section for this new context, i.e., experimentally determine which whole-number digital period value provides a corresponding period with the least amount of error, provide an appropriate screenshot of the relevant waveform with the minimal amount of error, including its <em>precise</em> frequency, and provide within the caption of the relevant screenshot the whole-number value that resulted in a minimal amount of error. Finally, describe</td>

       <td width="41"></td>

       <td width="348">and explain why there may be any differences between the two contexts, i.e., between using a prescaler value of 256 and a prescaler value of two.x. Create an assembly program to keep track of elapsing minutes with a timer/counter, i.e., design a “watch” that only has a “minute-hand”. (Hint: Instead of attempting to configure the period of the timer/counter to directly correspond to sixty seconds, configure the period to correspond to one second, and then keep track of how many times this timer/counter overflows [or underflows, if you wish to configure the timer/counter to count down].)</td>

      </tr>

     </tbody>

    </table><strong>4.</strong> <strong>LED ANIMATION CREATOR</strong></td>

  </tr>

 </tbody>

</table>

In this final part of the lab, you will design a comprehensive application that utilizes I/O components as well as timing mechanisms to create, edit, and display LED animations. The required program is as follows. (A skeleton file for the program is provided on our course website and is also available through the <em>Supplemental Materials </em>section of this document.)

Upon program start, your application should be in what will be referred to as <em>EDIT </em>mode. In this mode, the LEDs located on the <em>OOTB SLB</em> should be continually updated with the current logic values of the DIP switches also located on the <em>SLB</em>, just as in § 1. However, whenever tactile switch S1 on the <strong><em>SLB</em></strong> is determined to be depressed (i.e., pressed), the current logic values of the DIP switches should be stored into an 8-bit data memory table only <strong>once</strong>, starting at address 0x2000, where the value stored is to represent a “frame” of the LED animation. At any point during program runtime, the LED animation is defined to consist of only the animation frames currently stored in memory. Data memory addresses up through 0x3FFF should be allocated for the animation.

To guarantee that a value is stored only once per press of the relevant tactile switch, your program should wait for the switch to be released. Separately, it is likely that this tactile switch bounces (upon either a press or release of the switch), as almost all real, non-debounced switches do. To prevent multiple frames from being stored due to bouncing, it is will also be required that your program debounce this tactile switch. For the purposes of this lab, you may either perform this debouncing with a software delay or with timer/counter module. (Within the provided skeleton file, a method of debouncing using a timer/counter module is suggested.)

4.1. Read the pertinent sections of the <a href="https://mil.ufl.edu/3744/docs/switch_debouncing_through_software.pdf"><em>Switch Debouncing</em></a> <a href="https://mil.ufl.edu/3744/docs/switch_debouncing_through_software.pdf"><em>through Software</em></a> document. Note that, although a synchronous method for debouncing a switch with a timer/counter is not described in this document, the techniques described could be easily altered to account for synchronicity.

Your program should remain in <em>EDIT </em>mode until tactile switch S2 on the <strong><em>OOTB Memory Base </em></strong>[<strong>MB</strong>] is pressed. Note that it should not be necessary to debounce or wait for the release of this switch. When tactile switch S2 is determined to have been pressed, what is referred to as <em>PLAY </em>mode should commence.

In <em>PLAY </em>mode, the entire LED animation currently stored in memory should begin to play sequentially on your LEDs, starting from the initially stored frame. The rate at which each frame of the animation changes, or the “frame rate”, should be 20 Hz. (An error allowance of 3% is given.) Although software delays could be utilized to accomplish this frame rate, you <strong>must</strong> use a timer/counter. Upon reaching the end of the animation, the LED pattern should restart, and continue to be output until tactile switch S1 located on the <strong><em>OOTB MB </em></strong>(or <strong><em>OOTB EBIBB</em></strong>, if using a previous version of the lab kit) is pressed; after it is determined that this tactile switch has been pressed, the animation should stop, and the program should return to <em>EDIT </em>mode. Note that it should not be necessary to debounce or wait for the release of this switch.

Upon returning to <em>EDIT </em>mode, the program should function just as it did initially, although the LED animation previously stored should be retained, i.e., if tactile switch S1 on the <strong><em>SLB</em></strong> is pressed, another frame should simply be stored to the end of the previous LED animation. Thus, the program shall never terminate, and the only way to reset the LED animation should be to either re-program your microcontroller or to use the on-board reset button (labeled S1 on the <strong><em>µPAD</em></strong>).

Since you will need to debounce tactile switch S1 on the <em>OOTB SLB</em>, it will first be necessary to measure the amount of time that the switch bounces. To more easily measure the bouncing, you should perform measurements without the backpack connected to the <em>µPAD.</em>

4.2. If necessary, remove the <em>OOTB SLB </em>from the <em>µPAD</em>. Once the backpack is isolated, connect the appropriate voltage supplying pin from your DAD to one of the “3V3” pins on the Switch &amp; LED backpack, and connect the internal ground pin from the DAD to the “GND” pin on the backpack. Following this, provide 3.3 V to the backpack by using the <em>Supplies</em> feature within <em>Waveforms</em>. (If your DAD/NAD’s software is incapable of providing 3.3 V, it is also tolerable to power your backpack with 5 V.) Next, use your DAD and the <em>Scope </em>feature within <em>Waveforms</em> to record tactile switch bouncing and to determine an appropriate delay time. Refer to the available schematics, if necessary. Submit some screenshot(s) depicting the two relevant bouncing waveforms: one for the press of the switch, and one for the release of the switch.

<strong>NOTE: </strong>Many configurations within the <em>Scope </em>feature should be able to capture bouncing, although it may be helpful to use the <em>Repeated, Normal </em>mode, a trigger with a level of around 2 V, an initial timebase of around 25 ms (zooming in when determining if a bounce occurred), and any other appropriate configurations.




<table width="348">

 <tbody>

  <tr>

   <td width="348">of tactile switch S1 or tactile switch S2 located on the <em>OOTB MB </em>(or <em>OOTB EBIBB</em>, if using a previous version of the lab kit) – Why is this so?xii. Provide a scenario in which the above program would experience unintended behavior due to tactile switch bouncing.</td>

  </tr>

 </tbody>

</table>

4.3. Create        the      LED      animation      creation      program

(<strong>lab2_4.asm</strong>) described above. A skeleton file for the program is provided on our course website and is also available through the <em>Supplemental Materials </em>section of this document. <em>Your program should not be susceptible to tactile switch bouncing.</em>

<strong><u>PRE-LAB EXERCISES</u></strong>

<ol>

 <li>It is stated above that, in the relevant context, it should not be necessary to debounce (nor wait for the release of) either</li>

</ol>




<h2><u>PRE-LAB PROCEDURE SUMMARY</u></h2>

<ul>

 <li>Answer pre-lab exercises, when appropriate.</li>

 <li>Familiarize yourself with I/O ports in §1.</li>

 <li>Learn about software delays in § 2.</li>

 <li>Become introduced to timer/counter (TC) systems in § 3.</li>

 <li>Measure tactile switch bouncing and design an LED animation creator in § 4.</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> Relate this idea to the hypothetical thought of human life without sensory communication, i.e., no vision, sound, speech, smell, taste, etc.!

<a href="#_ftnref2" name="_ftn2">[2]</a> Ideally, the amount of memory used by these meaningless instructions would be small. To do so, some form of looping is generally preferred.

<a href="#_ftnref3" name="_ftn3">[3]</a> Assuming that the frame rate of the animation is 20 Hz, as it will be required in the pre-lab, and that your program stack does not interfere.