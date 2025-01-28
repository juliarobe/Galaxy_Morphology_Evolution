# Galaxy Morphology and Evolution
## Introduction:
Galaxies are the fundamental building blocks of the universe and come in a diverse range of shapes, sizes, and colors. Understanding the properties and evolution of galaxies is crucial for unraveling the mysteries of the cosmos. This project focuses on computing the luminosity, physical size, and surface brightness distribution of early-type (elliptical and lenticular) and late-type (spiral) galaxies using data from the Sloan Digital Sky Survey (SDSS). By analyzing the differences in these properties between galaxy types, we can gain insights into their formation and evolution, shedding light on the complex history of the universe. This project uses the SDSS g-band and r-band from Data Release 7.

## Downloading the data:
Before I begin analyzing galaxy data, I have to first retrieve it from the SDSS database. 

To do so, log into the <a href="[URL](https://casjobs.sdss.org/CasJobs/)">SDSS Database</a>. After logging in, we want to query the database for two samples of galaxies: one sample of Early-Type Galaxies (ETGs) and one sample of Late-Type Galaxies (LTGs). To obtain these samples, I use the following SQL queries. 

```
-- Query for Sample A (Early-type Galaxies)
SELECT TOP 5000
  gal.modelmag_g, gal.devmag_g, gal.expmag_g, gal.devrad_g, gal.exprad_g, gal.expab_g,
  gal.extinction_g, gal.z, gal.modelmag_r, gal.devmag_r, gal.expmag_r, gal.devrad_r, gal.expab_r,
  gal.extinction_r
FROM Galaxy AS gal
JOIN specObj AS sp
ON gal.specObjID=sp.specObjID
WHERE
  gal.petroR90_r/gal.petroR50_r > 2.4
  AND sp.eclass < 0
  AND gal.fracdev_g = 1


-- Query for Sample B (Late-type Galaxies)
SELECT TOP 5000
  gal.modelmag_g, gal.devmag_g, gal.expmag_g, gal.devrad_g, gal.exprad_g, gal.expab_g,
  gal.extinction_g, gal.z, gal.modelmag_r, gal.devmag_r, gal.expmag_r, gal.devrad_r, gal.expab_r,
  gal.extinction_r
FROM Galaxy AS gal
JOIN specObj AS sp
ON gal.specObjID=sp.specObjID
WHERE
  gal.petroR90_r/gal.petroR50_r < 2.2
  AND sp.eclass > 0.05
  AND (gal.fracdev_g BETWEEN 0 and 0.3)
```

After querying the SDSS database with SQL to retrieve galaxy parameters, I visually inspected the following samples of galaxy images using SDSS's SkyServer Explorer:
<h2 style="text-align: center;">Sample A: </h2>
<img class="img-fluid" style="display: block; margin: 0 auto;" src="../images/sampleA_color_images.PNG" width="500">
<h2 style="text-align: center;">Sample B: </h2>
<img class="img-fluid" style="display: block; margin: 0 auto;" src="../images/sampleB_color_images.PNG" width="500">



Sample A's galaxies appear redder in color, while Sample B's appear more blue. Sample A's galaxies also seem more dense with very little structure, while Sample B's are less dense and some appear to have more structure. This tells us that Sample A most likely consists of early-type galaxies such as elliptical and lenticular galaxies, while Sample B most likely contains later type galaxies like spiral galaxies. 

Next, I computed K-corrections, which account for the effects of redshift on galaxy brightness, by analyzing flux data from the SDSS database and applying filter responses and reddening corrections to galaxy spectra. This involved interpolating filter data to estimate responses at specific wavelengths, allowing me to visualize and understand the results.

