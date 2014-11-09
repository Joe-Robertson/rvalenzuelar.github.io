---
layout: post
title: "Weather radars: an animated example"
excerpt: "A brief description of Doppler weather radars"
tags: [doppler, radar, pacjet, fort ross, california, coastal mountain]
share: true
comments: true
---

Physical sciences are traditionally based on observations. For example, in atmospheric sciences there are a number of instruments that allow us to observe and learn about different parts of the atmosphere, from a simple liquid-in-glass thermometer to more complex instruments like radars (which stand for **RA**dio **D**etection **A**nd **R**anging).


<figure>
	<a href="/images/xpol.jpg"><img src="/images/xpol.jpg"></a>
	<figcaption>X-band dual polarization Doppler radar (X-POL) used by NOAA</figcaption>
</figure>

First radars were focused on military applications during the World War II. At the same time, radar operators realized that they were detecting not only aircrafts, but also other objects in the sky. After studying this issue, operators learned that these inconvenient signals returned from the distant weather, and therefore that radars could also be used in weather applications {% cite Rinehart Montopoli&Marzano %}.

In their most fundamental form, radars detect distant objects by sending a signal trough the air and then listening to any response. For example, if you release a stone into a water well and measure the time it takes until ear the sound of the water, then you can estimate the well's depth. Thus, you send a signal (the stone), receive a response (the sound of water) and can infer the depth because there is a physical constant (gravitational acceleration). In the case of radars, an antenna transmits an [electromagnetic](http://en.wikipedia.org/wiki/Electromagnetic_radiation) (EM) signal that travels at a constant speed trough the air (speed of light). If this signal interacts with any object in the air then the antenna receives a signal back and the distance to the object is computed using the [radar equation](http://en.wikipedia.org/wiki/Radar#Radar_equation). Of course, in reality there is much more detail on this, specially becuase all these processes are part of the [radiative transfer theory](http://en.wikipedia.org/wiki/Radiative_transfer).

As I mentioned before, radars were first developed for military propouses and then for scientific goals. These days radars are designed for mutiple applications {% cite Melvin&Scheer %}, and those focused on knowing the state of the atmosphere are called weather radars. Unlike the rest of the radars, weather radars are built with the detection of small atmospheric particles in mind, such as aerosols, cloud drops, and rain. Of course, at the end the signal transmited by the radar will detect any flying object (like bugs, birds, and aircrafts).

To detect and study the weather, radars create a signal using a part of the EM spectrum known as microwaves. Depending on the size and number of particles present in a volume of air, the signal can return information back to the radar or just pass through air. For example, thinny cloud particles will appear invisible to radars with large wavelengths (e.g. 10 cm), whereas large cloud drops and raindrops will attenuate radar signals with short wavelengths (e.g. 3 cm) and make the air volume behind these large particles appear "invisible" to the radar. 

<figure>
	<a href="/images/EM_spectrum.jpg"><img src="/images/EM_spectrum.jpg"></a>
	<figcaption>Electromagnetic spectrum (source: <a href="http://imagine.gsfc.nasa.gov/docs/science/know_l1/emspectrum.html">NASA</a> )</figcaption>
</figure>

 

A very basic weather radar would point its antenna to a fixed point in the sky and retrieve information along that single radar beam. However, radars usually have a mechanism to rotate the antenna using a scan strategy over a volume of the sky. This capability is of large importance to explore and understand structures in the circulation of the atmosphere, at least locally. 

The most common scan strategy is called plain-position indicator or PPI. I will not get into details about the origin of this name dated back to the first radars; however, I will explain how this scan is performed. If you stand stright and rise your arm in a 90° angle with respect to your torso, and assuming your arm represents a radar beam, then you would be using a 0° elevation angle. This would be your fixed elevation angle. Now, if you rise your arm to form 90.5° with respect to your torso, then the fixed elevation angle of the radar beam would be 0.5°. Finally, keeping this position, if you start rotating around yourself completing 360°, then you would be making a complete PPI scan at 0.5° elevation angle.

<figure>
	<a href="/images/ppi_scan.png"><img src="/images/ppi_scan.png"></a>
	<figcaption>Plan-position indicator (PPI) scan. The letter <i>e</i> indicates the fixed elevation angle. Adapted from {% cite Wood&Brown %}</figcaption>
</figure>

A basic function of radars is to detect the energy returned from distant objects, commonly known as echo. The physical quantity of this echo is represented by the energy received in the radar antenna (\\(P_r\\)) and in turn, we can associate this energy with a quantity called reflectivity factor (\\(z\\)):

$$z=k P_r r^2$$

In the above equation, \\(k\\) is a constant called *radar constant*. This constant is specific for each radar system and depends on, among other things, the wavelength employed and the energy transmitted by the radar. The value of \\(r\\) represents the range gate. A range gate is a fixed distance from the radar along the radar beam and can be determined, as explained above, using the speed of light as a physical constant. 

The key point here is that the reflectivity factor also represents the size and number of particles that exist in a volume of air and is simply expressed as:

$$z=\sum\limits_{i=1}^n D_i^6$$

This means that \\(z\\) is equal to the sum of the diameter \\(D\\) of each particle \\(i\\) existing in a volume of air (impressive, isn't it). Therefore, \\(z\\) is larger when there are more particles and/or when particles are larger in a given volume of air. Since the diameter of particles is raised to the sixth power, \\(z\\) is more sensitive to changes in the particle size (e.g. small changes in diameter produce significant changes in reflectivity factor). In addition, \\(z\\) assumes that all these particles are little spheres made of water. To make explicit this assumption, we add the adjective "equivalent", which means that \\(z\\) represents the reflectivity factor as if all the particles were liquid spheres.

The diameter of weather particles vary considerably in nature. For example, liquid particles like cloud drops can be as small as 10 \\(\mu m\\) and raindrops can vary from 100 to 1,000 \\(\mu m\\), whereas solid particles such as graupel and hail can range from 5,000 \\(\mu m\\) to 100,000 \\(\mu m\\) {% cite Rogers&Yau Prup&Klett %}. As you can see, these differences are so large that a logarithmic unit called decibel (dB) is needed. Then, you will normally see reflectivy factor values in units of dBZ.

In addition to reflectivity, modern radars have a system to detect the velocity at which particles are moving through the air. The physical principle dictates that if the EM signal returned to the radar has a different phase (a property of EM energy) compared with the signal emitted, then this difference will be proportional to the speed of the particle. This principle was stablished by [Christian Doppler](http://en.wikipedia.org/wiki/Christian_Doppler) and today is known as the Doppler effect. The Doppler effect principle is relatively simple to understand. Nevertheless, its interpretation is slighly more difficult with scanning radars due to the rotation of the antenna. I will get into more details about the Doppler velocity interperation in a future post. 

The following animation shows an example of a PPI scan made with a polarimetric X-band (i.e. wavelength of ~3 cm) scanning radar similar to the X-POL you see at the beginning of this article. Observations were made during a landfalling winter storm as part of a NOAA's field experiment called [Hydrometeorological Testbed](http://www.esrl.noaa.gov/psd/programs/2004/hmt/). The elevation angle is 0.5° and there is one scan every 6 minutes. The radar observes offshore and is located along the coast of northern California at Fort Ross.

<figure>
	<a href="/images/PPI_animation.gif"><img src="/images/PPI_animation.gif"></a>
	<figcaption>Animated example of a plan-position indicator (PPI) scan strategy</figcaption>
</figure>

In the left panel you see the Doppler velocity associated with the landfalling winter storm. Cold colors represent negative Doppler velocity values (m/s). This means that the airflow at each of these points is moving toward the radar along the radar beam. On the other hand, warm colors represent poisitve Doppler velocity values. This means that the airflow is moving away from the radat at each of these points along the radar beam. The right panel shows the equivalent radar reflectivity (dBZ). The more reddish the color, the largest the reflectivity and largest the cloud particles, which in this case can be interpreted as larger amounts of precipitation. In both panels the elevation of the coastal terrain is represented by black and white colors.

A book that I would recommend for starting in weather radar meteorology is [Radar for Meteorologist](http://www.radarwx.com/Welcome.html) by Ronald Rinehart. Although it avoids equations as much as possible, it provides the basics necessary to then access more quantitative-focused books, which are many out there.


References

{% bibliography --cited%}





























