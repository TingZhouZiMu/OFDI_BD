# OFDI_BD

```{stata}

// TF-IDF算法


gen lnws=ln(wordsum)
*每年年报的数量
bys year: egen nt=count(stkcd)


foreach w in 大数据 海量数据 数据中心 信息资产  数据化 算力{
	
	
	gen db_`w'= ln(`w'+1)
	gen tf_`w'=(1+`w')/(1+lnws) if `w'>0 & !missing(`w')
	replace tf_`w'=0 if `w'==0
	
	bys year: egen dft`w'=count(stkcd)  if `w'>0 & !missing(`w')
	replace dft`w'=0 if dft`w'==.
	bys year: egen dft`w'2 = max(dft`w')

	gen idf_`w'=ln(nt/dft`w'2)
	
	gen tf_idf_`w'=tf_`w'*idf_`w'
}


egen bd_tfidf =rowtotal(tf_idf_大数据 tf_idf_海量数据 tf_idf_数据中心 tf_idf_信息资产  tf_idf_数据化 tf_idf_算力)




```
