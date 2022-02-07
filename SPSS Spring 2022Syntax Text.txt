* Encoding: UTF-8.
show dir.
cd "Samples\English".
*other locations for different languages. 
*cd 'Samples\Japanese'
    cd 'Samples\Traditional Chinese'
    cd 'Samples\Spanish'.



GET
  FILE='car_sales.sav'.
DATASET NAME DataSet1 WINDOW=FRONT.

COMPUTE engine=RND(engine_s).
EXECUTE.

*graphs -> legacy dialog.
DATASET ACTIVATE DataSet1.
GRAPH
  /HISTOGRAM=price.



*analyze -> desriptive stats -> frequencies

FREQUENCIES VARIABLES=manufact type
  /ORDER=ANALYSIS.

*transform > compute variable. Paste below formula into wizard.
COMPUTE price_t=price + price*.08.
EXECUTE.

* analyze > descriptive stats > descriptive stats.
DESCRIPTIVES VARIABLES=width mpg price_t
  /STATISTICS=MEAN STDDEV MIN MAX.




*create scatterplots. graphs > legacy > scatter
    
DATASET ACTIVATE DataSet1.
GRAPH
  /SCATTERPLOT(BIVAR)=width WITH mpg
  /MISSING=LISTWISE.

GRAPH
  /SCATTERPLOT(BIVAR)=length WITH mpg
  /MISSING=LISTWISE.

*analyze > correlate > bivariate.
*Compute bivariate correlations for variables width, length, mpg.
CORRELATIONS
  /VARIABLES=width length mpg
  /PRINT=TWOTAIL NOSIG
  /MISSING=PAIRWISE.


*analyze > regression > linear. Explain output. 
REGRESSION
  /MISSING LISTWISE
  /STATISTICS COEFF OUTS R ANOVA
  /CRITERIA=PIN(.05) POUT(.10)
  /NOORIGIN 
  /DEPENDENT mpg
  /METHOD=ENTER width length.

*export output. output window. file > export > which format > pick location to save. 
* Export Output. Change location. 
*OUTPUT EXPORT
  /CONTENTS  EXPORT=ALL  LAYERS=PRINTSETTING  MODELVIEWS=PRINTSETTING
  /PDF  DOCUMENTFILE='C:\Users\PhotonUser\Downloads\fullOutput.pdf'
     EMBEDBOOKMARKS=YES  EMBEDFONTS=YES.

*analyze > descriptive stats > Crosstabs. Explain output. 
CROSSTABS
  /TABLES=engine BY type
  /FORMAT=AVALUE TABLES
  /STATISTICS=CHISQ 
  /CELLS=COUNT ROW COLUMN 
  /COUNT ROUND CELL.

