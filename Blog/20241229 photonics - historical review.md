
# References

Jacob B. Khurgin
- 2015: How to deal with the loss in plasmonics and metamaterials: https://www.nature.com/articles/nnano.2014.310
- 2022: Expanding the Photonic Palette: Exploring High Index Materials: https://pubs.acs.org/doi/full/10.1021/acsphotonics.1c01834
- 2023: Nonlinear optics from the viewpoint of interaction time: https://www.nature.com/articles/s41566-023-01191-3
- 2024: Energy and Power Requirements for Alteration of the Refractive Index: https://doi.org/10.1002/lpor.202300836

AWG:
- https://x.com/jwt0625/status/1830788722679521784
- Meint Smit: https://scholar.google.com/citations?user=6l3VOfgAAAAJ&hl=en&oi=sra
- New focusing and dispersive planar component based on an optical phased array: https://pure.tue.nl/ws/portalfiles/portal/4438613/590292.pdf

60s ~ 70s
- PK Tien
	- Tien1977: Integrated optics and new wave phenomena in optical wave guides 
		- https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.49.361
	- Tien1970: MODES OF PROPAGATING LIGHT WAVES IN THIN DEPOSITED SEMICONDUCTOR FILMS 
		- https://doi.org/10.1063/1.1652820
	- Tien1971: Light Waves in Thin Films and Integrated Optics
		- https://opg.optica.org/ao/fulltext.cfm?uri=ao-10-11-2395&id=72960


## Laser
https://history.aip.org/exhibits/laser/index.html
https://journals.aps.org/pr/abstract/10.1103/PhysRev.112.1940
https://www.hrl.com/news/2020/05/11/hrl-laboratories-celebrates-60th-anniversary-of-first-laser
![[Pasted image 20250110224441.png]]


## Fiber

Colladon, D. On the reflections of a ray of light inside a parabolic liquid stream. Comptes Rendus 15, 800–802 (1842)
- https://web.archive.org/web/20140222223427/http://www.ville-ge.ch/mhs/pdf/aide_colladon.pdf

Hopkins1954: A Flexible Fibrescope, using Static Scanning
- https://www.nature.com/articles/173039b0

Kao1966: Dielectric-fibre surface waveguides for optical frequencies
- https://digital-library.theiet.org/doi/10.1049/piee.1966.0189

CK Kao Nobel prize speech:
- https://www.nobelprize.org/prizes/physics/2009/kao/lecture/

## Planar lightwave circuit
Doerr and Okamoto

## SOI
- Soref1986: [All-silicon active and passive guided-wave components for λ = 1.3 and 1.6 µm](https://doi.org/10.1109/JQE.1986.1073057)

### Products:
- intel light peak: https://www.facebook.com/watch/?v=277370301433



## Coupling methods


Prism to chip:
- Tien1970: Theory of Prism–Film Coupler and Thin-Film Light Guides: https://opg.optica.org/josa/abstract.cfm?uri=josa-60-10-1325
- Ilchenko1999: Pigtailing the high-_Q_ microsphere cavity: a simple fiber coupler for optical whispering-gallery modes
	- https://doi.org/10.1364/OL.24.000723


Tapered fiber:
- Cai2000: Observation of Critical Coupling in a Fiber Taper to a Silica-Microsphere Whispering-Gallery Mode System: https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.85.74
- Gröblacher2013: Highly efficient coupling from an optical fiber to a nanoscale silicon optomechanical cavity
	- https://doi.org/10.1063/1.4826924

Edge coupler:
- https://ieeexplore.ieee.org/document/6895281


Review
- Marchetti2019: Coupling strategies for silicon photonics integrated chips (Invited) https://opg.optica.org/prj/fulltext.cfm?uri=prj-7-2-201&id=404538
- Edge coupler: https://www.mdpi.com/2076-3417/10/4/1538

Commentary:
- Rickman2014: [The commercialization of silicon photonics](https://www.nature.com/articles/nphoton.2014.175)
- Jones2012: [Myths and rumours of silicon photonics](https://www.nature.com/articles/nphoton.2012.66)
- Hochberg2010: [Towards fabless silicon photonics](https://www.nature.com/articles/nphoton.2010.172)


# Integration

Heidel2016: A Review of Electronic-Photonic Heterogeneous Integration at DARPA
- https://ieeexplore.ieee.org/document/7486128

# Ref

[Maxwell1865](https://royalsocietypublishing.org/doi/pdf/10.1098/rstl.1865.0008)

# Cover image
List of source:
- Colladon1842
- Hopkins1954
- 
- Maiman1960 (https://www.hrl.com/news/2020/05/11/hrl-laboratories-celebrates-60th-anniversary-of-first-laser)
- Berreman1965
- Tien1971
- Soref1986
- 
- Ilchenko1999
- Cai2000
- Waldhäusl1997
- Roelkens2005
- 
- Doerr2008
- Murakowski2024



# Adding interactive plot

2025-01-23T23:51:32-08:00
After some back and forth, end up using plotly.

Example:
```
  

# %%

import plotly.graph_objs as go

from plotly.subplots import make_subplots

  

fig = make_subplots()

fig.add_trace(go.Scatter(x=[1,2,3], y=[4,5,6]))

fig.update_layout(

sliders=[{

"steps": [{"args": [{"y": [i, i+1, i+2]}], "label": i} for i in range(10)]

}]

)

  

fig.write_html("../_includes/plot.html", include_plotlyjs="cdn")

# %%
```

- needed to use plotlyjs from CDN.

To include the generated html:
- `{% include plot.html %}`

