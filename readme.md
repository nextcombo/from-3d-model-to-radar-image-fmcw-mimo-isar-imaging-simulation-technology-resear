# From 3D Model to Radar Image: FMCW MIMO-ISAR Imaging Simulation Technology Research [with Python code]

From Three-Dimensional Models to Radar Images: Research on FMCW MIMO-ISAR Imaging Simulation Technology
Introduction: Inverse Synthetic Aperture Radar (ISAR) can achieve high-resolution imaging of non-cooperative targets such as aircraft, ships, missiles, and extract features like target size and structural shape. It is a core detection method in airspace situation awareness and anti-aircraft defense.
ISAR relies on broadband signals for high-resolution range imaging and utilizes the relative rotational motion between the radar and the target for high-resolution azimuth imaging. After completing lateral motion compensation with conventional single-station ISAR, it can be equivalently modeled as a turntable observation, and the classic Range-Doppler (RD) algorithm is used for two-dimensional imaging.
However, the azimuth resolution of traditional single-station ISAR is constrained by the physical antenna aperture, presenting inherent bottlenecks in imaging performance. MIMO (Multiple-Input Multiple-Output) radar introduces the virtual aperture theory, significantly enhancing angle resolution without increasing the number of physical antennas; it also improves azimuth resolution through waveform diversity by constructing an equivalent large aperture array. Time Division Multiplexing (TDM) is the mainstream implementation scheme, where the array coupling generates a vast amount of virtual elements equivalent to expanding the antenna observation aperture.
At the same time, Frequency Modulated Continuous Wave (FMCW) radar has natural advantages in miniaturization, lightweight, and low interception probability. Combining FMCW technology with MIMO-ISAR imaging can not only ensure high-resolution imaging capabilities but also reduce hardware costs and system complexity.
This paper constructs an end-to-end FMCW MIMO-ISAR full-linkage simulation framework, which supports the direct import of three-dimensional STL geometric models to generate high-fidelity radar images. It fully covers the entire processing process of scattering point extraction, multi-channel echo synthesis, two-dimensional image focusing, and imaging quality assessment.
The ISAR imaging basic principles and the rotary model of distance-Doppler algorithm are the theoretical foundations for ISAR imaging. In the actual observation scene, After the target translation component is eliminated by motion compensation, Approximately, it can be considered as a static target rotating around a fixed reference point at a constant speed.
Under this model, The Doppler shifts produced by the scattering points on the target surface are determined by their transverse position relative to the rotation center. Reconstruct the target's two-dimensional contour by analyzing the backscatter Doppler dimension information.
Distance - Doppler (RD) algorithm is the most classical imaging process of ISAR, which is divided into two steps:
1  
Distance compression: Perform a frequency domain transformation on the backscatter fast time-domain, outputting one-dimensional distance images corresponding to each pulse frame.
2  
Doppler Focusing: Conduct a Fourier transform along the slow time (pulse sequence dimension) to map the Doppler frequency onto the target's transverse coordinates, resulting in a two-dimensional radar image.
The distance resolution is uniquely determined by the emission signal bandwidth: where, To emit signal bandwidth. The azimuth resolution depends on the total rotational observation angle of the target and the radar operating wavelength.
The uniqueness of FMCW system can be utilized in traditional pulse ISAR with the ideal "on-off" approximation. However, due to the extremely long pulse width of FMCW signals, the target movement during the pulse duration cannot be ignored, leading to fast time - slow time coupling and causing one-dimensional range ambiguities, main lobe broadening, and ultimately severe image distortion.
FMCW radars generally employ a dechirp receiving architecture: the received echo is mixed with a local reference linear frequency-modulated signal, and the output difference frequency signal is obtained. The difference frequency has a linear relationship with the target distance.
Decoupling frequency modulation can significantly reduce the bandwidth of the back-end signal and ADC sampling pressure. Simplified hardware acquisition link; Performing Fourier transform on the difference frequency and intermediate frequency signals can achieve distance compression. However, the phase error caused by the fast-slow time coupling must be compensated in the imaging process. Otherwise, it is impossible to achieve effective focusing.
MIMO radar and virtual aperture technology MIMO radar core gain is derived from the construction of the virtual aperture array. System configuration of an emitting antenna. When a receiving antenna is used. By using waveform diversity orthogonal separation of the echoes from each transmit and receive channel. Equivalently generate an independent virtual element. The coordinates of virtual elements are calculated by coupling the geometrical positions of transmitting and receiving antennas. Equivalent aperture is much larger than the physical array size.
TDM Time Division Multiplexing is an MIMO working mode that is easily implemented in engineering: each transmitting antenna sends a frequency-modulated continuous wave in turn. All receiving antennas synchronized to collect echoes; The backend decouples multi-channel data based on emission timing. The architecture has a simple hardware structure. However, the constraint is that the target attitude must be within the complete TDM frame period. The electromagnetic scattering characteristics of the scene are approximately constant. The virtual aperture enlargement will improve the azimuth resolution performance. Simultaneously accompanied by gate fingers. Sub-effects such as redundant elements.
For FMCW MIMO imaging link, The echo processing flow is: each transmitting and receiving independent channel first completes the delineation of line frequency modulation distance compression. Slow time-of-flight preprocessing. Finally, all channels of the data are coherently superimposed. Coherent synthesis relies on the fixed phase relationship between channels. Synchronously achieve resolution enhancement and output signal-to-noise ratio gain.
Preliminary Steps from STL Model to Scattering Point Cloud Radar Simulation: Discreteization of 3D Geometric Model into Equivalent Electromagnetic Scattering Point Cloud. STL (Stereolithography) is a universal three-dimensional mesh storage format, which uses triangular facets to represent the geometric contours of an object's surface. The complete process of scattering point extraction is as follows:
1  
Triangular facet analysis and reading of STL files extracts the spatial coordinates and normal vectors of the triangular facets' three vertices. Based on the vertex coordinates, it calculates the area and geometric center coordinates of the facets.
2  
Scattering point spatial sampling is based on the area of a single facet and the custom sampling interval. Calculate the number of internal sampling points in the triangle facets. Randomly sample uniformly within the triangular facet region. The smaller the sampling interval, The higher the scattering point density, The more accurate the depiction of the target's fine geometric structure, Simulation computations have risen synchronously.
3  
The normal vector and scattering weight are assigned to each sampled scattering point, which inherits the normal vector of the corresponding facet. For subsequent occlusion discrimination. RCS scattering intensity calculation; The equivalent scatter area of a single scattering point is divided equally among all sampling points by the total surface area of the facet. Ensure the conservation of the total scattering cross-section.
4  
Model coordinate center preprocessing calculates the center of the target's 3D bounding box. Translate all scattered point coordinates to the origin of the coordinate system. Make the target symmetrically distributed around the origin of coordinates. Simplified subsequent rotations. Geometric distance calculation.
After the above steps, The continuous three-dimensional grid model is discretized into a set of scattering points. Each point carries three-dimensional coordinates. Surface normal Three categories of fundamental attributes for scattering weights. As the underlying input for radar echo simulation.
Backscatter signal synthesis FMCW signal model FMCW transmits linear frequency modulated signals. The instantaneous frequency changes linearly with time. Transmitted envelope expression: In the formula, denotes the carrier center frequency. To adjust the frequency slope. For signal bandwidth, For single pulse width.
The propagation delay of the backscattering point echo is: single-station radar delay; MIMO dual-station mode delay. After mixing and frequency detuning, the echo outputs a difference frequency, which corresponds to the target distance in size.
MIMO-TDM echo synthesis process: TDM-MIMO mechanism: single pulse at a time, only one transmit antenna is active, and the full receive array synchronizes to collect; a complete TDM frame includes group pulses, with each group pulse corresponding to multiple receive channels echoes from one transmit antenna.
Step-by-step echo simulation logic:
1  
Geometric rotation of scattering points traverses each slow time pulse. According to the total rotational angle observed. Calculate the current target turn angle using pulse index. Rotate all scattered points around the rotation axis. Simulate the relative radar rotational attitude of the target.
2  
Distance calculation for dual-station path, for the current active receiving and transmitting channel combination. Calculate the emission antenna to scattering point, The propagation distance from the scattering point to the receiving antenna. Total path; In a single station scenario, the transmission and reception distances are equal.
3  
Combined with the scattering point's normal vector, Determine the occlusion relationship of radar line-of-sight vector; The amplitude of scattered echoes and The cosine of the angle between the normal vectors is proportional. Set the backscattering contribution of radar facet to zero. Simulate the effect of target self-occlusion.
4  
The contribution of a single scattering point echo amplitude to the phase amplitude superposition: The first term is the carrier propagation phase, and the second term is the residual video phase (RVP) introduced by line frequency modulation. FMCW imaging must compensate for this phase error.
5  
Frequency domain energy accumulation interpolation distributes the backscattering energy of all scattering points according to the corresponding difference frequency to the spectral frequency points. Smooth energy distribution using linear interpolation. Reduce the quantization error of discrete frequency points.
The inverse Fourier transform is performed on the difference frequency spectrum obtained by accumulation. Recover the time-domain differential frequency signal; Frequency-domain signal with window (Hanning window / Hamming window) after suppressing sidelobes, then performs Fourier transform. Output the one-dimensional distance representation of distance compression.
Two-dimensional image formation distance - The single pulse one-dimensional distance image sequence obtained after Doppler two-dimensional focusing distance compression. Build a two-dimensional data matrix along the slow time dimension: The horizontal axis represents distance to the fast time. The vertical axis represents the pulse slow time.
Perform Fourier transform on the slow time dimension in a distance unit by unit. Map Doppler shifts to target lateral coordinates. In the turntable model, The horizontal velocity of scattering point is linearly related to the distance from the rotation center. Doppler frequency directly corresponds to the target's transverse spatial position; Before slow time-transformation, a window can be added to suppress sidelobes in the Doppler dimension.
MIMO multi-channel coherent synthesis completes the RD imaging of each transmit and receive channel independently, outputting multiple complex sub-images. There are two fusion schemes:
1  
Non-coherent synthesis only superimposes the amplitude / intensity of each channel image. Maximum projection. Simple calculation Robust against phase error. However, the phase information of lost channel is lost. The improvement in angular resolution is limited.
2  
Coherent synthesis directly performs the summation and superposition on all channel complex images. Make full use of the phase-coherent gain of virtual arrays. Achieving theoretically limit resolution performance; However, the phase consistency of the channel Motion compensation accuracy System calibration requirements are stringent.
In actual observation, the non-ideal rotation and irregular translation of the target will introduce random phase errors, causing image distortion. The phase disturbances are corrected by self-focusing algorithms, which mainly include two types:
1  
Phase Gradient Self-Focusing (PGA) Iterative Class High-Efficiency Self-Focusing Algorithm: Extraction of strong scattering peak at each distance unit and centralization. Windowing to capture the main lobe region. Solving phase gradients. The integral gives the global phase error. Phase compensation at each distance unit. Fast convergence rate. Adapted to most ISAR observation scenarios.
2  
The minimum entropy self-focusing is optimized by the image information entropy. Optimal phase correction quantity is solved by gradient descent iteratively. The lower the entropy value, the more concentrated the image energy is. The better the focus effect, Suitable for low signal-to-noise ratio and weak scatter target scenarios.
The imaging quality assessment indicators adopt quantitative metrics to quantify the radar image focusing and resolution performance:
1  
The measured 3dB main lobe width in the directional and angular resolutions is closer to the theoretical resolution, which indicates better imaging performance.
2  
Peak sidelobe ratio (PSLR) - The ratio of the main lobe peak intensity to the highest sidelobe peak. Measure sidelobe suppression capability; The higher the PSLR, Weakly scattered targets are not easily obscured by strong sidelobes.
3  
Integral sidelobe ratio (ISLR) is the ratio of total energy in the main lobe region to the total energy in the full sidelobe region, which characterizes the degree of image energy concentration.
4  
Image contrast image pixel intensity standard deviation to mean ratio, reflecting the image dynamic range, contour clarity.
5  
Virtual aperture fill factor MIMO array-specific metrics: number of independent virtual elements / theoretical maximum number of virtual elements. Quantify the utilization efficiency of a virtual aperture array. Guide the optimization of antenna array layout.
Simulation experiments and engineering application standard reference target verification: Verify the correctness of the simulation framework using known benchmark targets with analytical solutions.
Single Point Target: Simulation of point spread function (PSF), verifying the match between imaging peak coordinates, measured resolution, and theoretical values;
Double Point Target: Verify the distance resolution limit, measuring the error between two-point imaging separation and the actual geometrical distance;
Bisectrix Reflector: Verifying the accuracy of surface scattering and occlusion modeling, comparing the imaging contour with the actual geometrical shape.
Complex Three-Dimensional Object Imaging Simulation Imports STL mesh models of aircraft, ships, etc., completes scattering point sampling, FMCW-MIMO multi-channel echo simulation, and full-linkage imaging processing. The output images can clearly restore key structures such as the fuselage, wings, ship sides, and shipyards; compared to single station ISAR, MIMO virtual aperture array can significantly reduce the main lobe width in azimuth, enhancing detail resolution capability.
Array parameter traversal optimization simulation framework supports batch scanning experiments for antenna parameters: testing variables such as number of transmit/receive elements, element spacing, linear/surface array geometric configurations, etc., and outputs quantitative indicators like resolution, PSLR, and fill factor under different configurations for comparison, providing data support for actual radar array hardware design and system parameter selection.
This simulation platform can serve as a foundation for radar system parameter iteration optimization, new imaging algorithm verification, and generation of radar target recognition datasets, providing comprehensive simulation support for air defense and anti-missile, sea surveillance ISAR radar system design. It can be further expanded to include advanced simulation modules such as distributed multi-station radar network and three-dimensional ISAR 3D imaging, further improving the capabilities of the simulation system.

## Images

![From 3D Model to Radar Image: FMCW MIMO-ISAR Imaging Simulation Technology Research [with Python code]](cover.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
