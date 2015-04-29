# Knitr commands 
* To knit an R markdown document using the command line 
```bash
> Rscript -e "library(knitr); knit('kernelConvolution.Rmd')"
```

* To use accents in Rmd

include in the header

```
header-includes:
  - \usepackage[latin9]{inputenc}
```

Then on a mac insert the character of interest

Acute  ó Ó	Option+E, V

Circumflex	ô Ô	Option+I, V

Grave	ò Ò	Option+`, V	

Tilde	õ Õ	Option+N, V	Only works with "n,N,o,O,a,A"

Umlaut	ö Ö	Option+U, V
	

