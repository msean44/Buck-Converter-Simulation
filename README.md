# Buck-Converter-Simulation
This project is part of my end-of-year high school STEAM project. It's a Buck Converter Simulation that features a custom triangle-wave generator, comparator, and smoothing circuit.

## General Circuit
<img width="946" height="390" alt="image" src="https://github.com/user-attachments/assets/1eb5825d-835b-44ad-82a6-ab8481f1dbbb" />

The main circuit features:
- Differential Amplifier
- Triangle-wave generator (Astable Multivibrator fed into an integrator)
- Comparator
- DC-smoothing

### Differential Amplifier
The mechanism then flows onto the differential amplifier; it uses the principle of closed-loop gain. As we know, the voltage is available at both inputs, so it is a combination of inverting and non-inverting op-amp behavior. Therefore, the output voltage is given by the formula:

**V_out = A(V+ − V−)**

where *A* is the gain. In open-loop gain, the gain is determined by the specification of the op-amp used, or usually infinity. However, in this case, the gain can be determined by:

**A = Rf / Ri**

where *Rf* is the feedback resistor and *Ri* is the input resistor. The derivation will be provided in the findings. For the output to always be positive DC, the inverting input always has to be smaller than the non-inverting input. In the opposite case, the output will immediately go to ground, because the negative supply voltage is set to ground.

There needs to be calculations on how this component can determine whether the load battery is fully charged already.

<img width="593" height="193" alt="image" src="https://github.com/user-attachments/assets/117335b3-76cf-4c11-b1e8-7de6d0b52b03" />
<img width="624" height="493" alt="image" src="https://github.com/user-attachments/assets/14b0bbc0-b0a4-46d9-810b-83286f1531c3" />

### Triangle wave generator

The output voltage from the differential amplifier is then connected to the inverting input of the next op-amp. It is compared to a triangle wave generator, which is connected to the non-inverting input. The mechanism of the triangle wave generator includes similar components as the rest of the circuit, including op-amps. There are three steps: starting from an astable multivibrator, which acts as the square wave generator, then feeding into an integrator, which converts it into a triangle wave.

**Astable Multivibrator**

The mechanism starts with both the inverting and non-inverting inputs grounded. However, the output is fed back to both inputs; the non-inverting input is connected to a potential divider, and the inverting input is connected to an adjustable potentiometer and a capacitor.

Assuming the output voltage is positive at the beginning, the electrons from the capacitor will be attracted toward the output voltage, meaning current flows toward the capacitor, charging it. As it charges, the voltage at the inverting node of the op-amp builds up and becomes higher than the non-inverting input (assuming an ideal op-amp, so no current flows into the inputs). The output will now go negative, so the capacitor starts discharging toward the negative voltage; the voltage at the inverting node decreases while the voltage at the non-inverting node builds up (assuming *R2* must be bigger than *R1* to generate a higher voltage). In this way, the output voltage becomes positive again.

**Integrator**

The next step is the integrator, which acts as the converter from square-wave voltage to triangle-wave voltage. The concept used here is integration; it measures the area under the curve as the output. However, the output is inverted due to the derivation of the formula. Therefore, if the input is positive voltage, the output starts decreasing gradually; when the input changes to negative, the output starts increasing gradually, as shown in the triangle wave graph (Figure 6).

The working principle of the integrator is similar to the differential amplifier; the inverting op-amp tries to keep both of its inputs the same. However, the difference is that one of the resistors is replaced by a capacitor.

<img width="437" height="288" alt="image" src="https://github.com/user-attachments/assets/383c14a2-bee9-42f0-b9c6-9a55cf8df1e6" />

<img width="572" height="355" alt="image" src="https://github.com/user-attachments/assets/4690790e-b31f-4364-ba9d-fc876f463cc2" />

### PWM Generator
In the next step, the DC output voltage from the differential amplifier and the triangle wave voltage from the triangle wave generator are fed to another op-amp, where the non-inverting input comes from the differential amplifier and the inverting input comes from the triangle wave generator. This op-amp acts as a comparator, comparing both inputs and switching the supply voltage, depending on the higher input, as the output.

The output forms a PWM signal because the triangle wave voltage is always changing and becomes higher and lower than the DC voltage. This results in an output that continuously switches between the positive supply voltage and ground, as seen more clearly in the voltage-time graph (Figure 7).

Furthermore, the duty cycle of the PWM output can be changed depending on the value of the DC voltage from the differential amplifier. If the value increases, the duty cycle decreases because the duration where the triangle wave voltage is higher becomes shorter, so the output stays at the negative supply voltage (ground) for a longer duration. In contrast, if the DC voltage decreases, the duration where the triangle wave voltage is higher becomes longer, increasing the duration of the positive supply as the output.

Lastly, the transistor diode acts as a switch. It connects the supply voltage, the PWM voltage, and the step-down voltage converter. This component matches the duty cycle and frequency from the PWM to the supply voltage and relays it to the step-down voltage converter.

For instance, if the supply voltage is 12V with a PWM duty cycle of 50% and a frequency of 1kHz, the result is a chopped voltage with a maximum of 12V and a minimum of 0V at 1kHz. However, this assumes all components are ideal; in real cases, there may be an input offset voltage, causing the values to be off by a few millivolts.

<img width="365" height="227" alt="image" src="https://github.com/user-attachments/assets/bfaf51a2-ac13-4e17-9d81-bd4a374ae9c2" />

Example calculation: 

Duty cycle (D) = (Time ON / Period) × 100%

Time ON/Above: 7.2350 − 7.23455 = 0.45 ms

Time OFF/Below: 7.23555 − 7.2350 = 0.55 ms

D = (0.45 / 1.0) × 100 = 45%

### DC Smoothing
There are multiple components in use, such as a switch, diode, inductor, capacitor, and resistor. Components here each have different roles, starting with the switch, which is used to step down the frequency of the voltage coming in. If the switch is open and closed at a certain frequency, the voltage-time graph would be chopped with a maximum value of voltage when the switch is closed and zero when open. The higher the frequency of the switch, the lower the duty cycle will be, so the lower voltage will be produced. The addition of inductors and capacitors is for smoothing the stepped-down voltage to the battery to prevent damaging it. As a final input, the diode works to create the correct path for electrons and prevent any electrons from accumulating only in one place and causing failure.

<img width="812" height="364" alt="image" src="https://github.com/user-attachments/assets/3a0f263e-5ab3-4e04-a960-3a5228cdc5be" />


## Final Waveform
<img width="623" height="363" alt="image" src="https://github.com/user-attachments/assets/17d214c9-1d5c-4c66-ae81-c1614fc58a6d" />
