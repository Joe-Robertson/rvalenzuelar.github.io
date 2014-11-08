---
layout: post
title: "Weather radars: an animated example"
excerpt: "A brief description of Doppler weather radars"
tags: [doppler, radar, pacjet, fort ross, california, coastal mountain]
share: true
comments: true
---

Physical sciences are traditionally based on observations. For example, in atmospheric sciences there are a number of instruments that allow us to observe and learn about different parts of the atmosphere, from a simple thermometer to more complex instruments like radars (which stand for Radio Detection and Ranging).

First radars were focused on military applications during the World War II. Some years later, radar operators started to realize that radars were detecting not only aircrafts, but also other objects in the sky. After studying this "issue", operators learned that these non-conventional signals returned from the distant weather, and therefore that radars could also be used in weather applications.

Radars use the same principle by which timing the fall of a stone into a water well gives you an estimation of its depth. For example, the stone falls with a [constant linear acceleration](http://en.wikipedia.org/wiki/Equations_of_motion#Constant_linear_acceleration:_collinear_vectors) (~9.8 m/s2). Therefore, if you know the time between the release of the stone and the sound of the stone hitting the water surface, then you can easily compute the distance using Newton's equation of motion. In the case of a radar, the antenna transmits an [electromagnetic signal](http://en.wikipedia.org/wiki/Electromagnetic_radiation) (e.g. the stone) that travels at a constant speed trough the air (speed of light). If this signal interacts with some object (e.g. water surface in the well) then the antenna receives a signal back (e.g. the sound from the water surface) and the distance to the object is then computed using the [radar equation](http://www.radartutorial.eu/01.basics/The%20Radar%20Range%20Equation.en.html).

As I mentioned before, radars were first developed for military propouses and then for scientific goals. Usually, when radars are oriented for research or survillance of the atmosphere they are denominated weather radars. Unlike the rest of the radars, weather radars are built with the detection of small atmospheric particles in mind, such as aerosols, cloud drops, and rain. Of course, at the end the signal transmited by the radar will detect any flying object (like bugs, birds, and aircrafts). The key point is chosing the right electromagnetic signal for the right object.

In applications other than research and survillance of the atmosphere, radars focus on detecting relatively large objects and therefore radio waves are sufficient for estimating the location of aircrafts or ships at long distances and with a small loss of the signal (assuming ideal conditions). Nevertheless, detecting tiny atmospheric particles requeries signals borrowed from the electromagnetic spectrum known as microwaves. 

Depending on the size and number of particles present in a given air volume, the signal can return information back to the radar or just pass through air volume. For example, thinny cloud particles will appear invisible to radars with large wavelengths (e.g. 10 cm), whereas large raindrops will attenuate the signal of radars with short wavelengths (e.g. 3 cm) and make the air volume behind these large raindrops appear "invisible" to the radar. 



 



A very basic weather radar would point its antenna to a fixed point in the sky and retrieve information along that single radar beam. However, radars usually have a mechanism to rotate the antenna using a certain scan strategy over a volume of the sky. This capability is of large importance to explore and understand structures in the circulation of the atmosphere, at least locally. 

The most common scan strategy is called plain-position indicator or PPI. I will not get into details about the origin of this name (dated back to the first radars); however, I will explain how this scan is performed. If you stand stright and rise your arm in a 90 degree angle with respect to your torso, and assuming your arm represents a radar beam, then you would be using a 0 degree elevation angle. This would be your fixed elevation angle. Now, if you rise your arm to form 90.5 degrees with respect to your torso, then the fixed elevation angle of the radar beam would be 0.5 degrees. Finally, keeping this position, if you start rotating around yourself completing 360 degrees, then you would be making a complete PPI scan at 0.5 degrees elevation angle.

In addition to the radar's wavelength, many modern radars have a system to detect the velocity at which particles are moving through the air. The physical principle dictates that if the EM sent by the radar has a different phase once returned, then the difference in phase will be proportional to the speed of the particle. This principle was stablished by [Christian Doppler](http://en.wikipedia.org/wiki/Christian_Doppler) and today is known as the Doppler effect. 

The Doppler effect principle is relatively simple to understand. Nevertheless, its interpretation is slighly more difficult with scanning radars due to the rotation of the antenna. I will get into more details about the Doppler velocity interperation in a future post. Meanwhile, here I left an example of a PPI scan made at 0.5 degrees of elevation angle along the coast of northern California. 

<figure>
	<a href="/images/PPI_animation.gif"><img src="/images/PPI_animation.gif"></a>
	<figcaption>Example of a plan-position indicator (PPI) scan strategy</figcaption>
</figure>

In the left panel you see the Doppler velocity associated with the landfalling winter storm. Cold colors represent negative Doppler velocity values. This means that the airflow at each of these points is moving toward the radar along the beam. Warm colors represent poisitve Doppler velocity values. This means that the airflow is moving away from the radat at each of these points along the radar beam. The right panel shows the radar reflectivity. The more redish the colors, the largest the reflectivity, and largest the raindrops. In both panels the elevation of the coastal terrain is represented by black and white colors {% cite Rinehart%}.

A book that I would recommend for starting in weather radar meteorology is [Radar for Meteorologis](http://www.radarwx.com/Welcome.html) by Ronald Rinehart. It provides the basics necessary to access to more quantitative-focused books (there are many of them)


References

{% bibliography --cited%}





























