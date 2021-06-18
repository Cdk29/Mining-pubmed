Mining\_dependencies
================

Here I limite the set of sentences to the one containing sleep and
breakfast.

``` r
library(pubmedR)
library(udpipe)
```

## Functions

``` r
got_abstract <- function(query) {
  res <- pmQueryTotalCount(query = query, api_key = api_key)
  print(res$total_count)
  
  D <- pmApiRequest(query = query, limit = res$total_count, api_key = NULL)
  
  #From the xml-structured object to a “classical” data frame
  D <- pmApiRequest(query = query, limit = res$total_count, api_key = NULL)
  
  M <- pmApi2df(D)
  str(M)
  texts<-M$AB
  
  return(texts)
}
```

``` r
pipeline_abstract <- function(texts) {
  texts<-tolower(texts) #required first for stopwords lol
  abstracts<-data.frame("abstracts"=texts, "doc_id"=c(1:length(texts)))
  colnames(abstracts)
  return(abstracts)
}
```

``` r
ud_model <- udpipe_download_model(language = "english-gum")
```

    ## Downloading udpipe model from https://raw.githubusercontent.com/jwijffels/udpipe.models.ud.2.5/master/inst/udpipe-ud-2.5-191206/english-gum-ud-2.5-191206.udpipe to /home/erolland/Bureau/Mining_pubmed_abstract/english-gum-ud-2.5-191206.udpipe

    ##  - This model has been trained on version 2.5 of data from https://universaldependencies.org

    ##  - The model is distributed under the CC-BY-SA-NC license: https://creativecommons.org/licenses/by-nc-sa/4.0

    ##  - Visit https://github.com/jwijffels/udpipe.models.ud.2.5 for model license details.

    ##  - For a list of all models and their licenses (most models you can download with this package have either a CC-BY-SA or a CC-BY-SA-NC license) read the documentation at ?udpipe_download_model. For building your own models: visit the documentation by typing vignette('udpipe-train', package = 'udpipe')

    ## Downloading finished, model stored at '/home/erolland/Bureau/Mining_pubmed_abstract/english-gum-ud-2.5-191206.udpipe'

``` r
ud_model <- udpipe_load_model(ud_model$file_model)
```

``` r
query <- "breakfast*[Title/Abstract] AND sleep*[Title/Abstract] AND english[LA] AND Journal Article[PT]"
api_key = NULL
```

``` r
texts<-got_abstract(query)
```

    ## [1] 536
    ## Documents  200  of  536 
    ## Documents  400  of  536 
    ## Documents  536  of  536 
    ## Documents  200  of  536 
    ## Documents  400  of  536 
    ## Documents  536  of  536 
    ## ================================================================================
    ## 'data.frame':    535 obs. of  30 variables:
    ##  $ AU       : chr  "CURRENTI W;GODOS J;CASTELLANO S;CARUSO G;FERRI R;CARACI F;GROSSO G;GALVANO F" "SHIRVANI N;GHAFFARI M;RAKHSHANDEROU S" "LEE SJ;CARTMELL KB" "WU AC;RAUH MJ;DELUCA S;LEWIS M;ACKERMAN KE;BARRACK MT;HEIDERSCHEIT B;KRABAK BJ;ROBERTS WO;TENFORDE AS" ...
    ##  $ AF       : chr  "CURRENTI, WALTER;GODOS, JUSTYNA;CASTELLANO, SABRINA;CARUSO, GIUSEPPE;FERRI, RAFFAELE;CARACI, FILIPPO;GROSSO, GI"| __truncated__ "SHIRVANI, NASRIN;GHAFFARI, MOHTASHAM;RAKHSHANDEROU, SAKINEH" "LEE, SU JUNG;CARTMELL, KATHLEEN B" "WU, ALEXANDER C;RAUH, MITCHELL J;DELUCA, STEPHANIE;LEWIS, MARGO;ACKERMAN, KATHRYN E;BARRACK, MICHELLE T;HEIDERS"| __truncated__ ...
    ##  $ TI       : chr  "TIME-RESTRICTED FEEDING IS ASSOCIATED WITH MENTAL HEALTH IN ELDERLY ITALIAN ADULTS." "BREAKFAST CONSUMPTION-RELATED PERCEIVED BEHAVIOUR CONTROL AND SUBJECTIVE NORMS AMONG GIRL ADOLESCENTS: APPLYING"| __truncated__ "AN ASSOCIATION RULE MINING ANALYSIS OF LIFESTYLE BEHAVIORAL RISK FACTORS IN CANCER SURVIVORS WITH HIGH CARDIOVA"| __truncated__ "RUNNING-RELATED INJURIES IN MIDDLE SCHOOL CROSS COUNTRY RUNNERS: PREVALENCE AND CHARACTERISTICS OF COMMON INJURIES." ...
    ##  $ SO       : chr  "CHRONOBIOLOGY INTERNATIONAL" "JOURNAL OF EDUCATION AND HEALTH PROMOTION" "JOURNAL OF PERSONALIZED MEDICINE" "PM & R : THE JOURNAL OF INJURY, FUNCTION, AND REHABILITATION" ...
    ##  $ SO_CO    : chr  "ENGLAND" "INDIA" "SWITZERLAND" "UNITED STATES" ...
    ##  $ LA       : chr  "ENG" "ENG" "ENG" "ENG" ...
    ##  $ DT       : chr  "JOURNAL ARTICLE" "JOURNAL ARTICLE" "JOURNAL ARTICLE" "JOURNAL ARTICLE" ...
    ##  $ DE       : chr  "MEDITERRANEAN;TIME-RESTRICTED FEEDING;AGING;BRAIN DISEASES;CHRONONUTRITION;COHORT;INTERMITTENT FASTING;MENTAL HEALTH" "ADOLESCENTS;BREAKFAST;FEMALE;PERCEIVED BEHAVIOUR CONTROL;SUBJECTIVE NORM'S" "ASSOCIATION RULE MINING;CANCER SURVIVOR;CARDIOVASCULAR DISEASE;HEALTH RISK ASSESSMENT;LIFESTYLE RISK BEHAVIOR" "" ...
    ##  $ ID       : chr  "" "" "" "" ...
    ##  $ MESH     : chr  "" "" "" "" ...
    ##  $ AB       : chr  "IN RECENT YEARS, MENTAL DISORDERS HAVE REPRESENTED A RELEVANT PUBLIC HEALTH PROBLEM DUE TO THEIR DELETERIOUS EF"| __truncated__ "HEALTHY NUTRITION IN CHILDHOOD AND ADOLESCENCE IS IMPORTANT FOR GROWTH AND DEVELOPMENT. BREAKFAST IS THE MOST I"| __truncated__ "WE AIMED TO ASSESS WHICH LIFESTYLE RISK BEHAVIORS HAVE THE GREATEST INFLUENCE ON THE RISK OF CARDIOVASCULAR DIS"| __truncated__ "UNDERSTANDING THE PREVALENCE AND FACTORS ASSOCIATED WITH RUNNING-RELATED INJURIES IN MIDDLE SCHOOL RUNNERS MAY "| __truncated__ ...
    ##  $ C1       : chr  "DEPARTMENT OF BIOMEDICAL AND BIOTECHNOLOGICAL SCIENCES, UNIVERSITY OF CATANIA, CATANIA, ITALY.;OASI RESEARCH IN"| __truncated__ "MSC OF HEALTH EDUCATION AND HEALTH PROMOTION, SCHOOL OF PUBLIC HEALTH AND SAFETY, SHAHID BEHESHTI UNIVERSITY OF"| __truncated__ "RESEARCH INSTITUTE ON NURSING SCIENCE, SCHOOL OF NURSING, HALLYM UNIVERSITY, 1 HALLYMDAEHAK-GIL, CHUNCHEON-SI 2"| __truncated__ "SPAULDING REHABILITATION HOSPITAL, CHARLESTOWN MA; HARVARD MEDICAL SCHOOL, BOSTON, MA, USA.;DOCTOR OF PHYSICAL "| __truncated__ ...
    ##  $ CR       : chr  "NA" "NA" "NA" "NA" ...
    ##  $ TC       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ SN       : chr  "1525-6073" "2277-9531" "2075-4426" "1934-1563" ...
    ##  $ J9       : chr  "CHRONOBIOL INT" "J EDUC HEALTH PROMOT" "J PERS MED" "PM R" ...
    ##  $ JI       : chr  "CHRONOBIOL INT" "J EDUC HEALTH PROMOT" "J PERS MED" "PM R" ...
    ##  $ PY       : num  2021 2020 2021 2020 2021 ...
    ##  $ PY_IS    : chr  "2021" "2021" "2021" "2021" ...
    ##  $ VL       : chr  NA "10" "11" NA ...
    ##  $ DI       : chr  "10.1080/07420528.2021.1932998" "10.4103/jehp.jehp_474_20" "10.3390/jpm11050366" "10.1002/pmrj.12649" ...
    ##  $ PG       : chr  "1-10" "96" NA NA ...
    ##  $ GRANT_ID : chr  "" "" "HRF 202005-008" "" ...
    ##  $ GRANT_ORG: chr  "" "" "HALLYM UNIVERSITY RESEARCH FUND" "" ...
    ##  $ UT       : chr  "34100325" "34084843" "34063255" "34053194" ...
    ##  $ PMID     : chr  "34100325" "34084843" "34063255" "34053194" ...
    ##  $ DB       : chr  "PUBMED" "PUBMED" "PUBMED" "PUBMED" ...
    ##  $ AU_UN    : chr  "DEPARTMENT OF BIOMEDICAL AND BIOTECHNOLOGICAL SCIENCES, UNIVERSITY OF CATANIA, CATANIA, ITALY.;OASI RESEARCH IN"| __truncated__ "MSC OF HEALTH EDUCATION AND HEALTH PROMOTION, SCHOOL OF PUBLIC HEALTH AND SAFETY, SHAHID BEHESHTI UNIVERSITY OF"| __truncated__ "RESEARCH INSTITUTE ON NURSING SCIENCE, SCHOOL OF NURSING, HALLYM UNIVERSITY, 1 HALLYMDAEHAK-GIL, CHUNCHEON-SI 2"| __truncated__ "SPAULDING REHABILITATION HOSPITAL, CHARLESTOWN MA; HARVARD MEDICAL SCHOOL, BOSTON, MA, USA.;DOCTOR OF PHYSICAL "| __truncated__ ...
    ##  $ AU_CO    : chr  "NA" "NA" "NA" "NA" ...
    ##  $ AU1_CO   : chr  "NA" "NA" "NA" "NA" ...

``` r
abstracts<-pipeline_abstract(texts)


x <- udpipe_annotate(ud_model, x = abstracts$abstracts, doc_id = abstracts$doc_id)
x <- as.data.frame(x)

head(x)
```

    ##   doc_id paragraph_id sentence_id
    ## 1      1            1           1
    ## 2      1            1           1
    ## 3      1            1           1
    ## 4      1            1           1
    ## 5      1            1           1
    ## 6      1            1           1
    ##                                                                                                                                                                         sentence
    ## 1 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ## 2 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ## 3 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ## 4 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ## 5 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ## 6 in recent years, mental disorders have represented a relevant public health problem due to their deleterious effect on quality of life and the difficulty of timely diagnosis.
    ##   token_id     token    lemma  upos xpos       feats head_token_id dep_rel deps
    ## 1        1        in       in   ADP   IN        <NA>             3    case <NA>
    ## 2        2    recent   recent   ADJ   JJ  Degree=Pos             3    amod <NA>
    ## 3        3     years     year  NOUN  NNS Number=Plur             8     obl <NA>
    ## 4        4         ,        , PUNCT    ,        <NA>             3   punct <NA>
    ## 5        5    mental   mental   ADJ   JJ  Degree=Pos             6    amod <NA>
    ## 6        6 disorders disorder  NOUN  NNS Number=Plur             8   nsubj <NA>
    ##            misc
    ## 1          <NA>
    ## 2          <NA>
    ## 3 SpaceAfter=No
    ## 4          <NA>
    ## 5          <NA>
    ## 6          <NA>

``` r
colnames(x)
```

    ##  [1] "doc_id"        "paragraph_id"  "sentence_id"   "sentence"     
    ##  [5] "token_id"      "token"         "lemma"         "upos"         
    ##  [9] "xpos"          "feats"         "head_token_id" "dep_rel"      
    ## [13] "deps"          "misc"

## Filtering sentences

``` r
#idx_sleep<-grep("sleep", x$sentence) 
idx_sleep<-grep("sleep duration", x$sentence)
idx_breakfast<-grep("breakfast", x$sentence) 

idx_both<-idx_sleep[which(idx_sleep %in% idx_breakfast)]
unique(x$sentence[idx_both])
```

    ##  [1] "there was a significant relationship between students' sleep duration and the people with whom they eat breakfast with the motivation to comply ( = 0.009), ( = 0.001) and subjective norms ( = 0.004), ( = 0.001) as well as between the people with whom they eat breakfast and normative beliefs ( = 0.05)."                                                                                                                                                                                                                                                                                                             
    ##  [2] "there was a significant relationship between father's job and control beliefs ( = 0.03) and perceived behavioural control ( = 0.04), between household size with perceived behavioural control ( = 0.05), between sleep duration and perceived power ( < 0.001), and perceived behavioural control ( = 0.03), between the people with whom they eat breakfast with control beliefs ( < 0.001), perceived power ( < 0.001), and perceived behavioural control ( < 0.001)."                                                                                                                                                   
    ##  [3] "considering the importance of sleep duration for adolescent girls as well as eating breakfast with other family members, health policymakers are recommended to pay special attention to these two factors while designing educational interventions."                                                                                                                                                                                                                                                                                                                                                                      
    ##  [4] "we used association rule mining to analyze patterns of lifestyle risk behaviors by ascvds risk group, based upon public health recommendations described in the alameda 7 health behaviors (current smoking, heavy drinking, physical inactivity, obesity, breakfast skipping, frequent snacking, and suboptimal sleep duration)."                                                                                                                                                                                                                                                                                          
    ##  [5] "the only parallel between sexes was that both female and male students with 3-5 times weekly breakfast were less likely to have short sleep duration."                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
    ##  [6] "among men, moderate/low physical activity, breakfast skipping, non-adequate breakfast duration, number of eating occasions and eating breakfast alone/depending on the occasion were associated with excess bf, while among women, low meddietscore, moderate/high alcohol consumption, non-adequate sleep duration, eating breakfast and lunch alone/depending on the occasion."                                                                                                                                                                                                                                           
    ##  [7] "always staying at home/working from home was associated with an increase in animal product, vegetable, fruit, mushroom, nut, water, and snack intake, as well as sleep duration and frequency of skipping breakfast (odds ratio (or) 1.54, 1.62, 1.58, 1.53, 1.57, 1.52, 1.77, 2.29, and 1.76 respectively)."                                                                                                                                                                                                                                                                                                               
    ##  [8] "students reported their lifestyle factors including sleep duration, time spent on computer, time spent on television, time spent on homework, eating breakfast, smoking, drinking, physical activity, and outdoor activity."                                                                                                                                                                                                                                                                                                                                                                                                
    ##  [9] "multivariate logistic regression analysis revealed that sleep duration <8 hour/day, time spent on homework ⩾3 hour/day, skipping breakfast, alcohol use, physical activity <3 days/week, and outdoor activity <2 hour/day were positively associated with depressive symptoms in both girls and boys."                                                                                                                                                                                                                                                                                                                      
    ## [10] "adolescents with breakfast, fruits, vegetables, milk and soft drinks intake were more likely to have longer sleep duration (p <0.05 for all)."                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
    ## [11] "this cross-sectional study investigated the prevalence of depression and its association with sleep patterns, eating habits and body weight status among a convenience sample of 527 adolescents, ages 10-17 years in mumbai, india. participants completed a survey on sleep patterns such as sleep duration, daytime sleepiness and sleep problems and eating habits such as frequency of breakfast consumption, eating family meals and eating out."                                                                                                                                                                     
    ## [12] "questionnaires were used to assess parameters such as moderate-to-vigorous physical activity per day, school and weekend night sleep durations, social jetlag, daytime sleepiness, napping, screen time, and breakfast intake."                                                                                                                                                                                                                                                                                                                                                                                             
    ## [13] "logistic regression analysis showed that the prevalence of depressive symptoms in the highest quartile of physical activity was lower than in the lowest quartile when adjusted for sex, age, ethnicity, only child, smoking status, alcohol use, breakfast frequency, daily sleep duration, body mass index, grip strength, and the number of metabolic syndrome components (odds ratios [95% confidence intervals (ci)]: 0.75 [0.58, 0.98], = 0.036)."                                                                                                                                                                    
    ## [14] "no significant differences were found for daily breakfast, sleep duration and parenting practices in adjusted analyses."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
    ## [15] "multinomial logistic regression was used to estimate ethnic differences in disruptors, such as skipping breakfast, eating erratically, and sleep duration."                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
    ## [16] "ethnic minority populations skipped breakfast more often, timed meals differently, had longer periods of fasting, ate more erratically, and had more short/long sleep durations than the dutch."                                                                                                                                                                                                                                                                                                                                                                                                                            
    ## [17] "information about sleep duration, daily consumption of eggs, milk, and breakfast were obtained from a self-administrated questionnaire."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
    ## [18] "however, no significant relationship was observed between the parameters of sleep duration, the interval between dinner and night sleep, consuming breakfast and snack during the day and nafld (all p > 0.05)."                                                                                                                                                                                                                                                                                                                                                                                                            
    ## [19] "the intervention had a minor negative effect on physical activity levels in boys at 12-month follow-up and short-term small-to-moderate positive effects on consumption of breakfast and fruit and vegetables, and sleep duration on school days."                                                                                                                                                                                                                                                                                                                                                                          
    ## [20] "in univariate analysis, being in 5 or 6 grade, frequent snacking, short sleep duration, long hours of media use, paternal smoking, and parental skipping of breakfast were significantly associated with many caries."                                                                                                                                                                                                                                                                                                                                                                                                      
    ## [21] "circadian rhythm factors were determined with a self-report questionnaire and included breakfast frequency, sleep duration, and work time."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
    ## [22] "however, there have been few studies reporting on the association of sleep duration with breakfast intake frequency."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
    ## [23] "this study examined the prevalence of nocturnal sleep duration among saudi children and its association with breakfast intake, screen time, physical activity levels and socio-demographic variables. a multistage stratified cluster random sampling technique was used to select 1051 elementary school children in riyadh."                                                                                                                                                                                                                                                                                              
    ## [24] "the sleep duration, daily breakfast intake frequency, socio-demographic and lifestyle behaviors were assessed using a specifically designed self-reported questionnaire filled by the children's parents."                                                                                                                                                                                                                                                                                                                                                                                                                  
    ## [25] "results of logistic regression analysis, adjusted for confounders, exhibited significant associations between longer sleep duration and younger age (aor=1.12, =0.046), being female (aor=1.39, =0.037), higher father educational levels, daily breakfast intake (aor=1.44, =0.049) and lower screen time (aor for >2 hrs/day=0.69, =0.033)."                                                                                                                                                                                                                                                                              
    ## [26] "health-related factors (sleep duration, frequency of breakfast) and working type in korean workers may affect the prevalence of mets."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
    ## [27] "here we assess if ebrbs adopted by adolescents included in a subsample are associated with markers of total and abdominal adiposity in a multicentre european study, healthy lifestyle in europe by nutrition in adolescence (helena-css) and a brazilian study, brazilian cardiovascular adolescent health (bracah study), and whether sleep duration influence the association between skipping breakfast, physical activity and sedentary behaviours, with total and abdominal obesity (ao)."                                                                                                                            
    ## [28] "skipping breakfast was associated with total and ao in adolescents independent of sleep duration."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
    ## [29] "after adjusting for age, exercise, smoking, sleep duration, and employment, the multivariable-adjusted odds ratios (ors) and 95% confidence intervals (cis) of skipping breakfast were 2.47 (2.18-2.81) for having a late dinner and 1.71 (1.53-1.91) for having a bedtime snack."                                                                                                                                                                                                                                                                                                                                          
    ## [30] "the important univariate risk factors for the outcome were dimensional measures of age, sex, breakfast consumption, experience of violence, sleep duration, perceived stress, feeling of sadness, current cigarette smoking, current alcohol drinking, perceived general health, perceived academic record, household economic status and living with biological or adoptive parents."                                                                                                                                                                                                                                      
    ## [31] "we found negative and significant partial correlations between regular consumption of breakfast and depressive symptoms, and between regular consumption of green and yellow vegetables and depressive symptoms in both junior and senior high school students, after controlling for age, sex, and sleep duration."                                                                                                                                                                                                                                                                                                        
    ## [32] "daily behaviours such as active commuting to school (acs) could be a source of physical activity, contributing to the improvement of youth cardiovascular health, however, the relationship between acs and other aspects of a youth's health, such as sleep duration and breakfast consumption, require further clarification."                                                                                                                                                                                                                                                                                            
    ## [33] "the aims of this study were therefore: 1) to analyse the prevalence of modes of commuting to school, sleep duration, and breakfast consumption by age groups and gender, and 2) to analyse the association between acs, sleep duration recommendations, and breakfast consumption by age groups and gender."                                                                                                                                                                                                                                                                                                                
    ## [34] "modes of commuting to/from school, sleep duration, and breakfast consumption were self-reported."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
    ## [35] "logistic regression models were fitted to examine the association between acs, sleep duration and breakfast consumption, analysed according to age groups and gender."                                                                                                                                                                                                                                                                                                                                                                                                                                                      
    ## [36] "the percentage of students meeting sleep duration and daily breakfast recommendations was lowest in older adolescents, and highest in children (6.3% versus 50.8% p < 0.001, and 62.1%, versus 76.8%, p = 0.001, respectively)."                                                                                                                                                                                                                                                                                                                                                                                            
    ## [37] "children (10-12 yr) were those that best meet with the adequate sleep duration and breakfast consumption recommendations."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
    ## [38] "insufficient sleep duration was associated with unhealthy dietary habits such as skipping breakfast (odds ratio [or] 1.30, 95% confidence interval [ci] 1.25-1.35), fast-food consumption (or 1.35, 95% ci 1.29-1.41), and consuming sweets regularly (or 1.32, 95% ci 1.25-1.39)."                                                                                                                                                                                                                                                                                                                                         
    ## [39] "the factors such as female, obesity, metabolic syndrome, current smoker, and skipping breakfast were positively associated with vitamin d deficiency, but high intensity of physical activity and more than 9 hours of sleep duration were negatively associated with vitamin d deficiency (all < 0.05)."                                                                                                                                                                                                                                                                                                                   
    ## [40] "we aimed to assess the dietary habit, time spent on social media, and sleep duration relationship to body mass index (bmi) among medical students in tabuk, saudi arabia. a cross-sectional study was conducted among 147 clinical phase medical students in the medical college, university of tabuk (saudi arabia) from january 2018 to may 2018. a checklist questionnaire was used to measure variables such as age, sex, smoking, level of exercise, whether taking meals and snacks regularly, eating fast food, fruit and vegetable consumption, sleep duration, time spent on social media, and breakfast skipping."
    ## [41] "participants consisted of 51% males, mean age (mean ± sd) was 22.90±1.27 years, sleep duration was 7.50±2.17 hours, time spent on social media was 5.54±3.49 hours, body mass index was 24.8±5.19, and breakfast skipping, fast food consumption, smoking, and regular exercise were reported in 52.4%, 87.7%, 12.9%, and 36.1% respectively. a significant negative correlation was evident between bmi and sleep duration (r= -0.185, p=0.025), cigarette smokers were more likely to be obese compared to their counterparts (27.28±6.85 vs. 24.10±4.98, p=0.018)."                                                      
    ## [42] "no significant statistical relationship was evident between bmi, breakfast skipping, fast food, fruit and vegetable intake, and time spent on social media. bmi was higher among smokers and those with shorter sleep duration, there was no association between bmi and other students' characteristics."                                                                                                                                                                                                                                                                                                                  
    ## [43] "this study assesses socio-economic health inequalities (sehi) over primary school-age (4- to 12-years old) across 13 outcomes (i.e. body-mass index [bmi], handgrip strength, cardiovascular fitness, current physical conditions, moderate to vigorous physical activity, sleep duration, daily fruit and vegetable consumption, daily breakfast, exposure to smoking, mental strengths and difficulties, self-efficacy, school absenteeism and learning disabilities), covering four health domains (i.e. physical health, health behaviour, mental health and academic health)."                                         
    ## [44] "boys who skipped breakfast and had short night-time sleep duration (≤6 h per night) were more likely to have poor health status."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
    ## [45] "girls who skipped breakfast, and had night-time eating patterns, personal computer use >4 h per day, and short night-time sleep duration (≤6 h/night) were more likely to have poor health status."                                                                                                                                                                                                                                                                                                                                                                                                                         
    ## [46] "late-night-dinner (standardized regression coefficient = 0.13, p = 0.028) was associated with hemoglobin a1c after adjusting for age, bmi, sex, duration of diabetes, smoking, exercise, alcohol, snacking after dinner, nighttime sleep duration, time from dinner to bedtime, skipping breakfast, and medication for diabetes."                                                                                                                                                                                                                                                                                           
    ## [47] "of the 10726 students, roughly 40% reported sleep duration <8 h. longer sleep duration was associated with higher likelihood of milk intake, fruit consumption, vegetable consumption, water consumption, moderate physical activity, and muscle-strengthening physical activity, and with a lower likelihood of cigarette use, alcohol use, sweets intake, western fast food intake, and breakfast skipping."                                                                                                                                                                                                              
    ## [48] "playing electronic games was positively associated with snacking at night and less frequently eating breakfast, and negatively associated with sleep duration and self-esteem."                                                                                                                                                                                                                                                                                                                                                                                                                                             
    ## [49] "height and weight were objectively measured and excess weight was defined in accordance with world health organization criteria. information on screen time, breakfast, physical activity and sleep duration was obtained through a self-administered questionnaire."                                                                                                                                                                                                                                                                                                                                                       
    ## [50] "totally irregular bm were significantly correlated with skipping breakfast (or, 1.30), slow eating (or, 1.41), physical inactivity (or, 1.27), long tv viewing (or, 1.52), late bedtime (or, 1.43), and short sleep duration (or, 1.33)."                                                                                                                                                                                                                                                                                                                                                                                   
    ## [51] "more frequent vegetable and milk consumption, greater physical activity, and longer sleep duration were positively associated with daily breakfast consumption, while soft drinks and fast food consumption, computer use, cigarette-smoking and alcohol-drinking behaviors were inversely associated."                                                                                                                                                                                                                                                                                                                     
    ## [52] "logistic regression analysis indicated that the odds ratios for experiencing intense feelings of anger were significantly higher (all p values < .05) among students who smoked, consumed alcohol, skipped breakfast, did not wish to go to university, had short sleep duration, had decreased positive feelings, had increased depressive feelings, or used mobile phones for longer hours."                                                                                                                                                                                                                              
    ## [53] "the odds ratios for experiencing intense impulsivity were significantly higher among students who smoked, consumed alcohol, skipped breakfast, did not participate in club activities, had short sleep duration, had decreased positive feelings, had increased depressive feelings, or used mobile phones for longer hours."                                                                                                                                                                                                                                                                                               
    ## [54] "participants provided information on physical activity, sedentary behavior, chronotype, sleep duration, sleep quality, and the consumption of breakfast, fish, and caffeine via a survey."                                                                                                                                                                                                                                                                                                                                                                                                                                  
    ## [55] "longer breakfast-to-lunch intervals were also associated with greater waist circumferences (β =-.35, p = .002) after adjusting for age, sex, and sleep duration."                                                                                                                                                                                                                                                                                                                                                                                                                                                           
    ## [56] "sleep duration, breakfast intake, parental age and education and paternal bmi did not have a consistently significant effect on physical fitness."                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
    ## [57] "the prevalence of metabolic syndrome is increasing worldwide, and previous studies have shown that inadequate sleep duration and skipping breakfast may be related to metabolic syndrome."                                                                                                                                                                                                                                                                                                                                                                                                                                  
    ## [58] "the sample included 12,999 subjects who participated in the knhanes iv & v. sleep duration and breakfast eating were self-reported, and metabolic syndrome was defined according to the modified national cholesterol education program adult treatment panel iii guidelines."                                                                                                                                                                                                                                                                                                                                              
    ## [59] "subjects were divided into 12 groups according to breakfast eating and sleep duration patterns, and multiple logistic regression analyses adjusted for age, sex, household income, education level, smoking status, alcohol drinking, physical activity, and total daily energy intake were conducted."                                                                                                                                                                                                                                                                                                                     
    ## [60] "while the consumption of breakfast (both genders) and milk (boys only) was positively associated with sleep duration (p<0.05)."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             
    ## [61] "to assess to what extent eight behavioural health risks related to breakfast and food consumption and five behavioural health risks related to physical activity, screen time and sleep duration are present among schoolchildren, and to examine whether health-risk behaviours are associated with obesity."                                                                                                                                                                                                                                                                                                              
    ## [62] "after multivariate adjustments, the severity of depressive symptoms was significantly associated with body mass index, leisure-time physical activity, current smoking, sleep duration, sucrose intake, skipping breakfast, insulin use, severe hypoglycemia, dysesthesia of both feet, history of foot ulcer, photocoagulation, ischemic heart disease, and stroke."                                                                                                                                                                                                                                                       
    ## [63] "personal and lifestyle data for the children (birth weight, type of breastfeeding, sleep duration, skipping breakfast, snacking, physical activity) and parents (ethnicity, educational level, occupation, weight, height) were collected by means of a questionnaire."                                                                                                                                                                                                                                                                                                                                                     
    ## [64] "partial mediation was found for sugared drinks intake and sleep duration in the greek sample, and breakfast in the dutch sample. a suppression effect was found for engagement in sports activites in the greek sample."                                                                                                                                                                                                                                                                                                                                                                                                    
    ## [65] "ethnic differences in children's body composition were partially mediated by differences in breakfast skipping in the netherlands and sugared drinks intake, sports participation and sleep duration in greece."                                                                                                                                                                                                                                                                                                                                                                                                            
    ## [66] "the possible mediating effect of sugared drinks intake, breakfast consumption, active transportation to school, sports participation, tv viewing, computer use and sleep duration in the association between parental education and children's body composition was explored via mackinnon's product-of-coefficients test in single and multiple mediation models."                                                                                                                                                                                                                                                         
    ## [67] "the factors associated with disorders of arousal were the grade in school, smoking habit, alcohol consumption, naptime (min), breakfast habit, participation in club activities, sleep duration, difficulty initiating sleep, nocturnal awakening, early morning awakening, subjective sleep assessment, snoring, decrease in positive feelings, and depression (all p<.001)."                                                                                                                                                                                                                                              
    ## [68] "furthermore, having low intake of breakfast (<3 day/week compared with 5 days or more per week) decreased the odd of having adequate sleep duration by a factor of 0.795 (95% ci = 0.667-0.947, p < 0.010)."                                                                                                                                                                                                                                                                                                                                                                                                                
    ## [69] "except for breakfast consumption at home, school-provided lunches, nighttime sleep duration, household and child routines did not predict stability or change in weight status."                                                                                                                                                                                                                                                                                                                                                                                                                                            
    ## [70] "dietary intakes, breakfast patterns and eating frequency, physical activity levels, sleep duration, anthropometric and physical examination data, biochemical indices and socioeconomic information (collected from parents) were assessed in all children."                                                                                                                                                                                                                                                                                                                                                                
    ## [71] "boys reported healthier habits concerning sleep duration, physical activity, eating breakfast, and smoking compared to the girls."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
    ## [72] "the confounding factors-'breakfast with family', 'watching television at dinner', 'eating and drinking before sleep', 'watching television for >2 h', 'sleep duration <8 h' and 'playing sports"                                                                                                                                                                                                                                                                                                                                                                                                                            
    ## [73] "health behaviors including smoking status, alcohol consumption, frequency of exercise, sleep duration, dietary habits (supplement use, breakfast, late-night snacking, eating regularly, and eating out), and self-rated health were obtained from a self-administered questionnaire at baseline."                                                                                                                                                                                                                                                                                                                          
    ## [74] "data were collected on the frequency of consumption of certain foods, physical activity patterns, sedentary habits at home, sleep duration and behaviors such as habits of snacking, skipping breakfast, eating in front of television and frequency of eating out."                                                                                                                                                                                                                                                                                                                                                        
    ## [75] "increased consumption of bakery items, non vegetarian foods, increased television viewing, decreased sleep duration, eating while watching television, snacking between meals, family meals, skipping breakfast (in older children), and parental bmi were found to be related to waist circumference."                                                                                                                                                                                                                                                                                                                     
    ## [76] "in all, 1146 children were recruited, from which 55 % took part in the study. a total of 604 9-11-year-old children (312 girls, 292 boys) were measured by research staff and completed a study questionnaire on their health behaviors, including breakfast intake, tv viewing, sleep duration and physical activity, and a 16-item food frequency questionnaire."                                                                                                                                                                                                                                                         
    ## [77] "lifestyle factors (overall/extracurricular physical activity, television viewing, reading as a hobby, sleep duration, breakfast/fruit intake, smoking and alcohol intake) as well as mode and duration of commuting to school were self-reported."                                                                                                                                                                                                                                                                                                                                                                          
    ## [78] "the analysis showed that only adequate sleep duration (or = 1·35, 95 % ci 1·11, 1·66; p = 0·003) and breakfast consumption (or = 0·66, 95 % ci 0·49, 0·87; p = 0·004) were independently associated with active commuting to school."                                                                                                                                                                                                                                                                                                                                                                                       
    ## [79] "preference for fatty food, skipping breakfast, and eating out were significantly associated with short sleep duration."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
    ## [80] "hierarchic logistic regression analyses were conducted to test the significance of the association between sleep duration and the incidence of obesity, before and after controlling for covariates, including dietary patterns (preference for fatty food, skipping breakfast, snacking, and eating out)."                                                                                                                                                                                                                                                                                                                 
    ## [81] "preference for fatty food, skipping breakfast, snacking, and eating out only partially explained the effects of short sleep duration on the incidence of obesity, suggesting that other factors, including physiologic mechanisms, may largely explain the sleep-obesity association."                                                                                                                                                                                                                                                                                                                                      
    ## [82] "weighted multiple logistic regression was used to identify the relationship between obesity at wave ii and sleep duration, having adjusted for skipping breakfast ≥ 2/week; race, gender, parental income, tv ≥ 2 h per day, depression, and obesity at wave i. at wave i, the mean age was 15.96 ± 0.11 years; mean sleep hours were 7.91 ± 0.04."                                                                                                                                                                                                                                                                         
    ## [83] "parental overweight, skipping breakfast, eating quickly, excessive eating, long hours of tv watching, long hours of video game playing, physical inactivity, and short sleep duration were associated with adolescent overweight."                                                                                                                                                                                                                                                                                                                                                                                          
    ## [84] "after adjusting for demographic and familial factors at baseline, the results showed that later bedtime [odds ratio (or) = 1.17, p = 0.043], later waking time (or = 1.19, p = 0.039), short sleep duration (or = 1.15, p = 0.061), physical inactivity (or = 1.48, p = 0.022), skipping breakfast (or = 1.56, p = 0.003), irregular snacks (or = 1.43, p < 0.001), and frequent instant noodle consumption (or = 1.49, p = 0.007) in early childhood increased the risk of poor qol in first-year jhss."

``` r
dim(x)
```

    ## [1] 149164     14

``` r
x<-x[idx_both,]
dim(x)
```

    ## [1] 3888   14

## Looking at token ID

``` r
grep_idx_head <- function(x, idx) {
  idx_head<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$token_id==x[idx,]$head_token_id)
  return(idx_head)
}


grep_dependencie <- function(x, idx_head, vector_heads) {
  #nice datastructure is required here
  #x[idx_head,]$token
  vector_heads<-c(vector_heads, x[idx_head,]$token)
  if (x[idx_head,]$head_token_id==0) {
    return(vector_heads)
  }
  idx_head_of_head<-grep_idx_head(x, idx_head)
  return(grep_dependencie(x, idx_head_of_head, vector_heads))
}
```

Tree is probably not the best name, chain of head would be more
suitable.

``` r
got_tree <- function(x, idx) {
  
  vector_heads<-c()
  idx_head<-grep_idx_head(x, idx)
  vector_heads<-grep_dependencie(x, idx_head, vector_heads)
  
}
```

``` r
idx_sleep<-which(x$token=="sleep")
vector_heads<-c()

list_vector_head<-list()
for (idx in idx_sleep) {
  #print(idx)
  #got_tree(x, idx)
  print(try(got_tree(x, idx)))
  
}
```

    ## [1] "students"     "relationship" "was"         
    ## [1] "duration"  "size"      "perceived" "was"      
    ## [1] "duration"    "importance"  "considering"
    ## [1] "duration"        "smoking"         "behaviors"       "alameda"        
    ## [5] "described"       "recommendations" "based"           "used"           
    ## [1] "duration" "have"     "likely"   "times"    "female"  
    ## [1] "duration"   "women"      "associated" "breakfast"  "eating"    
    ## [6] "occasions"  "eating"     "number"     "activity"  
    ## [1] "duration"   "increase"   "associated"
    ## [1] "including" "duration"  "factors"   "reported" 
    ## [1] "revealed"
    ## [1] "duration" "have"     "likely"  
    ## [1] "patterns"     "association"  "depression"   "prevalence"   "investigated"
    ## [1] "survey"       "completed"    "ages"         "investigated"
    ## [1] "duration"     "patterns"     "sleep"        "survey"       "completed"   
    ## [6] "ages"         "investigated"
    ## [1] "problems"     "."            "duration"     "patterns"     "sleep"       
    ## [6] "survey"       "completed"    "ages"         "investigated"
    ## [1] "durations"  "activity"   "parameters" "assess"     "used"      
    ## [1] "status"   "smoking"  "adjusted" "lower"    "showed"  
    ## [1] "found"
    ## [1] "eating"      "breakfast"   "differences" "estimate"    "used"       
    ## [1] "durations" "had"       "ate"      
    ## [1] "duration"    "information"
    ## [1] "duration"   "parameters" "observed"  
    ## [1] "dinner"   "interval" "observed"
    ## [1] "had"
    ## [1] "duration"   "grade"      "associated"
    ## [1] "determined"
    ## [1] "duration"    "association" "reporting"   "studies"     "have"       
    ## [1] "duration"   "prevalence" "examined"  
    ## [1] "duration" "assessed"
    ## [1] "duration"     "associations" "exhibited"    "results"      "female"      
    ## [6] "levels"      
    ## [1] "factors" "affect" 
    ## [1] "associated" "assess"    
    ## [1] "duration"    "independent" "adolescents" "total"       "associated" 
    ## [1] "duration"  "2.47"      "2.18-2.81" "adjusting"
    ## Error in if (x[idx_head, ]$head_token_id == 0) { : 
    ##   l'argument est de longueur nulle
    ## [1] "Error in if (x[idx_head, ]$head_token_id == 0) { : \n  l'argument est de longueur nulle\n"
    ## attr(,"class")
    ## [1] "try-error"
    ## attr(,"condition")
    ## <simpleError in if (x[idx_head, ]$head_token_id == 0) {    return(vector_heads)}: l'argument est de longueur nulle>
    ## [1] "found"
    ## [1] "duration"     "aspects"      "acs"          "relationship" "require"     
    ## [6] "source"      
    ## [1] "1"
    ## [1] "1"
    ## Error in if (x[idx_head, ]$head_token_id == 0) { : 
    ##   l'argument est de longueur nulle
    ## [1] "Error in if (x[idx_head, ]$head_token_id == 0) { : \n  l'argument est de longueur nulle\n"
    ## attr(,"class")
    ## [1] "try-error"
    ## attr(,"condition")
    ## <simpleError in if (x[idx_head, ]$head_token_id == 0) {    return(vector_heads)}: l'argument est de longueur nulle>
    ## [1] "examine" "fitted" 
    ## [1] "meeting"    "percentage"
    ## [1] "duration" "meet"     "those"   
    ## [1] "duration"   "associated"
    ## [1] "duration"   "hours"      "activity"   "intensity"  "associated"
    ## [1] "aimed"
    ##  [1] "food"      "eating"    "taking"    "age"       "variables" "measure"  
    ##  [7] "used"      "conducted" "sleep"     "aimed"    
    ## [1] "hours"     "years"     "males"     "consisted"
    ## [1] "duration"  "bmi"       "evident"   "spent"     "time"      "hours"    
    ## [7] "years"     "males"     "consisted"
    ## [1] "duration" "those"    "higher"   "evident" 
    ## [1] "vigorous"   "moderate"   "conditions" "index"      "assesses"  
    ## [1] "duration" "had"      "skipped"  "boys"     "likely"  
    ## [1] "duration"   "patterns"   "eating"     "night-time" "had"       
    ## [6] "girls"      "likely"    
    ## [1] "duration"   "dinner"     "snacking"   "associated"
    ## [1] "reported" "students" "sleep"   
    ## Error in if (x[idx_head, ]$head_token_id == 0) { : 
    ##   l'argument est de longueur nulle
    ## [1] "Error in if (x[idx_head, ]$head_token_id == 0) { : \n  l'argument est de longueur nulle\n"
    ## attr(,"class")
    ## [1] "try-error"
    ## attr(,"condition")
    ## <simpleError in if (x[idx_head, ]$head_token_id == 0) {    return(vector_heads)}: l'argument est de longueur nulle>
    ## [1] "duration"   "associated" "breakfast"  "eating"     "night"     
    ## [6] "snacking"   "associated"
    ## [1] "defined"  "measured"
    ## [1] "duration"   "breakfast"  "skipping"   "correlated"
    ## [1] "activity"   "associated"
    ## [1] "duration"  "had"       "indicated"
    ## [1] "duration"    "decreased"   "had"         "participate"
    ## [1] "provided"
    ## [1] "duration" "sleep"    "provided"
    ## [1] "associated"
    ## Error in if (x[idx_head, ]$head_token_id == 0) { : 
    ##   l'argument est de longueur nulle
    ## [1] "Error in if (x[idx_head, ]$head_token_id == 0) { : \n  l'argument est de longueur nulle\n"
    ## attr(,"class")
    ## [1] "try-error"
    ## attr(,"condition")
    ## <simpleError in if (x[idx_head, ]$head_token_id == 0) {    return(vector_heads)}: l'argument est de longueur nulle>
    ## [1] "duration"   "related"    "shown"      "increasing"
    ## [1] "participated" "subjects"     "included"    
    ## [1] "eating"    "breakfast" "according" "conducted" "divided"  
    ## [1] "duration"   "associated"
    ## [1] "time"      "duration"  "present"   "breakfast" "related"   "risks"    
    ## [7] "assess"   
    ## [1] "duration"   "activity"   "mass"       "associated"
    ## [1] "weight" "data"  
    ## [1] "intake" "found" 
    ## [1] "intake"      "sugared"     "skipping"    "breakfast"   "differences"
    ## [6] "mediated"   
    ## [1] "duration"  "explored"  "drinks"    "effect"    "mediating" "possible" 
    ## [1] "habit"   "smoking" "grade"  
    ## [1] "initiating" "difficulty" "duration"   "sleep"      "habit"     
    ## [6] "smoking"    "grade"     
    ## [1] "assessment" "sleep"      "initiating" "difficulty" "duration"  
    ## [6] "sleep"      "habit"      "smoking"    "grade"     
    ## [1] "duration"  "having"    "odd"       "decreased"
    ## [1] "duration" "predict" 
    ## [1] "intakes"  "assessed"
    ## [1] "duration"   "concerning" "habits"     "reported"  
    ## [1] "eating"             "dinner"             "watching"          
    ## [4] "family"             "factors-'breakfast"
    ## [1] "h"                  "sleep"              "eating"            
    ## [4] "dinner"             "watching"           "family"            
    ## [7] "factors-'breakfast"
    ## [1] "status"    "behaviors"
    ## Error in if (x[idx_head, ]$head_token_id == 0) { : 
    ##   l'argument est de longueur nulle
    ## [1] "Error in if (x[idx_head, ]$head_token_id == 0) { : \n  l'argument est de longueur nulle\n"
    ## attr(,"class")
    ## [1] "try-error"
    ## attr(,"condition")
    ## <simpleError in if (x[idx_head, ]$head_token_id == 0) {    return(vector_heads)}: l'argument est de longueur nulle>
    ## [1] "duration"    "decreased"   "consumption"
    ## [1] "intake"    "behaviors" "completed" "measured"  "recruited"
    ## [1] "viewing"       "television"    "activity"      "factors"      
    ## [5] "self-reported"
    ## [1] "duration"   "associated" "showed"    
    ## [1] "duration"   "associated"
    ## [1] "duration"     "association"  "significance" "test"         "conducted"   
    ## [1] "duration"   "effects"    "explained"  "eating"     "skipping"  
    ## [6] "preference"
    ## [1] "ii"           "obesity"      "relationship" "identify"     "used"        
    ## [1] "hours"    "7.91"     "i"        "h"        "tv"       "race"     "adjusted"
    ## [8] "used"    
    ## [1] "duration"   "playing"    "watching"   "eating"     "eating"    
    ## [6] "overweight"
    ## [1] "duration"   "inactivity" "increased"  "showed"

``` r
idx_breakfast<-which(x$token=="breakfast")
vector_heads<-c()

list_vector_head<-list()
for (idx in idx_breakfast) {
  #print(idx)
  #got_tree(x, idx)
  print(try(got_tree(x, idx)))
  
}
```

    ## [1] "eat"          "people"       "sleep"        "students"     "relationship"
    ## [6] "was"         
    ## [1] "eat"    "people" "as"     "was"   
    ## [1] "eat"       "people"    "control"   "perceived" "power"     "perceived"
    ## [7] "was"      
    ## [1] "eating"      "recommended" "girls"       "considering"
    ## [1] "skipping"        "smoking"         "behaviors"       "alameda"        
    ## [5] "described"       "recommendations" "based"           "used"           
    ## [1] "likely" "times"  "female"
    ## [1] "skipping" "activity"
    ## [1] "duration" "activity"
    ## [1] "eating"    "occasions" "eating"    "number"    "activity" 
    ## [1] "eating"     "women"      "associated" "breakfast"  "eating"    
    ## [6] "occasions"  "eating"     "number"     "activity"  
    ## [1] "skipping"   "duration"   "increase"   "associated"
    ## [1] "eating"   "spent"    "time"     "time"     "time"     "duration" "factors" 
    ## [8] "reported"
    ## [1] "skipping" "spent"    "time"     "sleep"    "revealed"
    ## [1] "adolescents" "intake"      "likely"     
    ##  [1] "consumption"  "frequency"    "habits"       "eating"       "problems"    
    ##  [6] "."            "duration"     "patterns"     "sleep"        "survey"      
    ## [11] "completed"    "ages"         "investigated"
    ## [1] "intake"     "activity"   "parameters" "assess"     "used"      
    ## [1] "frequency" "status"    "smoking"   "adjusted"  "lower"     "showed"   
    ## [1] "found"
    ## [1] "differences" "estimate"    "used"       
    ## [1] "more"    "often"   "skipped" "ate"    
    ## [1] "obtained"    "duration"    "information"
    ## [1] "consuming" "interval"  "observed" 
    ## [1] "consumption" "effects"     "follow-up"   "boys"        "levels"     
    ## [6] "effect"      "had"        
    ## [1] "skipping"   "grade"      "associated"
    ## [1] "frequency"     "included"      "questionnaire" "determined"   
    ## [1] "frequency"   "duration"    "association" "reporting"   "studies"    
    ## [6] "have"       
    ## [1] "intake"      "association" "children"    "examined"   
    ## [1] "intake"    "frequency" "duration"  "assessed" 
    ## [1] "intake" "levels"
    ## [1] "frequency" "duration"  "sleep"     "factors"   "affect"   
    ## [1] "skipping"    "association" "duration"    "sleep"       "associated" 
    ## [6] "assess"     
    ## [1] "associated"
    ## [1] "skipping"  "intervals" "duration"  "2.47"      "2.18-2.81" "adjusting"
    ## [1] "consumption" "age"         "measures"    "sleep"      
    ## [1] "consumption"  "correlations" "found"       
    ## [1] "consumption"  "duration"     "aspects"      "acs"          "relationship"
    ## [6] "require"      "source"      
    ## [1] "consumption" "sleep"       "1"          
    ## [1] "consumption"     "recommendations" "sleep"           "1"              
    ## [1] "consumption"   "duration"      "self-reported" "sleep"        
    ## [1] "consumption" "duration"    "sleep"       "examine"     "fitted"     
    ## [1] "recommendations" "duration"        "lowest"          "sleep"          
    ## [5] "meeting"         "percentage"     
    ## [1] "consumption"     "recommendations" "duration"        "meet"           
    ## [5] "those"          
    ## [1] "habits"     "associated"
    ## [1] "skipping"   "female"     "factors"    "associated"
    ##  [1] "skipping"  "media"     "spent"     "time"      "duration"  "sleep"    
    ##  [7] "food"      "eating"    "taking"    "age"       "variables" "measure"  
    ## [13] "used"      "conducted" "sleep"     "aimed"    
    ## [1] "skipping"  "hours"     "reported"  "spent"     "time"      "hours"    
    ## [7] "years"     "males"     "consisted"
    ## [1] "skipping" "bmi"      "evident" 
    ## [1] "exposure"   "duration"   "sleep"      "vigorous"   "moderate"  
    ## [6] "conditions" "index"      "assesses"  
    ## [1] "skipped" "boys"    "likely" 
    ## [1] "skipped" "girls"   "likely" 
    ## [1] "skipping"   "dinner"     "snacking"   "associated"
    ## [1] "skipping" "intake"   "sweets"   "sleep"   
    ## [1] "eating"     "night"      "snacking"   "associated"
    ## [1] "time"        "information" "defined"     "measured"   
    ## [1] "skipping"   "correlated"
    ## [1] "consumption" "associated" 
    ## [1] "consumed"  "smoked"    "students"  "higher"    "indicated"
    ## [1] "skipped"     "higher"      "participate"
    ## [1] "consumption" "duration"    "sleep"       "provided"   
    ## [1] "intake"   "duration" "have"     "sleep"   
    ## [1] "duration"   "related"    "shown"      "increasing"
    ## [1] "eating"        "duration"      "self-reported" "sleep"        
    ## [5] "participated"  "subjects"      "included"     
    ## [1] "according" "conducted" "divided"  
    ## [1] "consumption" "associated" 
    ## [1] "related" "risks"   "assess" 
    ## [1] "skipping"   "activity"   "mass"       "associated"
    ## [1] "skipping"  "duration"  "collected" "sleep"     "weight"    "data"     
    ## [1] "sample" "intake" "found" 
    ## [1] "differences" "mediated"   
    ## [1] "consumption" "intake"      "duration"    "explored"    "drinks"     
    ## [6] "effect"      "mediating"   "possible"   
    ## [1] "habit"   "habit"   "smoking" "grade"  
    ## [1] "intake"    "having"    "decreased"
    ## [1] "consumption" "predict"    
    ## [1] "patterns" "intakes"  "assessed"
    ## [1] "eating"   "reported"
    ## [1] "use"       "duration"  "sleep"     "status"    "behaviors"
    ## [1] "skipping" "snacking" "habits"   "duration" "sleep"   
    ## [1] "skipping"    "found"       "meals"       "snacking"    "watching"   
    ## [6] "eating"      "consumption"
    ## [1] "intake"    "behaviors" "completed" "measured"  "recruited"
    ## [1] "consumption" "="           "duration"    "associated"  "showed"     
    ## [1] "skipping"   "preference" "associated"
    ## [1] "skipping"   "preference" "patterns"   "conducted" 
    ## [1] "skipping"   "preference"
    ## [1] "2/week"   "skipping" "adjusted" "used"    
    ## [1] "skipping"   "overweight"
    ## [1] "skipping"   "inactivity" "increased"  "showed"

Plotting in the form of a tree could be considered.

## Other approaches of using head token

Who have head token id than sleep ?

``` r
idx<-121
x[idx,]$head_token_id
```

    ## [1] "60"

``` r
x[idx,]$doc_id
```

    ## [1] "2"

``` r
x[idx,]$sentence_id
```

    ## [1] 8

``` r
#for the token with same head token
idx_same_token_id<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$head_token_id==x[idx,]$head_token_id)

x[idx_same_token_id,]$token
```

    ## [1] ","       "between" "the"     "eat"

``` r
head_token<-x[grep_idx_head(x, idx),]$token
```

``` r
for (idx in idx_breakfast) {
  #print(idx)
  #got_tree(x, idx)
  idx_same_token_id<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$head_token_id==x[idx,]$head_token_id)
  head_token<-x[grep_idx_head(x, idx),]$token
  print(head_token)
  print("Associated to")
  print(x[idx_same_token_id,]$token)

}
```

    ## [1] "eat"
    ## [1] "Associated to"
    ## [1] "they"       "breakfast"  "motivation" "comply"    
    ## [1] "eat"
    ## [1] "Associated to"
    ## [1] "they"      "breakfast"
    ## [1] "eat"
    ## [1] "Associated to"
    ## [1] "whom"      "they"      "breakfast" "beliefs"  
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] "breakfast" "members"   ","        
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "likely"
    ## [1] "Associated to"
    ## [1] "breakfast" "were"      "less"      "have"     
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "duration"
    ## [1] "Associated to"
    ## [1] ","            "non-adequate" "breakfast"   
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] "and"       "breakfast"
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "lunch"    
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] "of"        "breakfast"
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "adolescents"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "of"        "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "frequency"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "found"
    ## [1] "Associated to"
    ## [1] "differences" "were"        "breakfast"   "sleep"       "."          
    ## [1] "differences"
    ## [1] "Associated to"
    ## [1] "ethnic"     "disruptors" "breakfast" 
    ## [1] "more"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "obtained"
    ## [1] "Associated to"
    ## [1] ","             "and"           "breakfast"     "were"         
    ## [5] "questionnaire"
    ## [1] "consuming"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "day"      
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "on"        "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "and"       "parental"  "breakfast"
    ## [1] "frequency"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "frequency"
    ## [1] "Associated to"
    ## [1] "with"      "breakfast" "intake"   
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] "with"      "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] ","         "daily"     "breakfast" "aor=1.44" 
    ## [1] "frequency"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] "between"   "breakfast"
    ## [1] "associated"
    ## [1] "Associated to"
    ## [1] "breakfast" "was"       "total"     "."        
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] "of"        "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "between"   "regular"   "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "and"       "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "duration"  ","         "and"       "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "and"       "breakfast"
    ## [1] "recommendations"
    ## [1] "Associated to"
    ## [1] "and"       "daily"     "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "habits"
    ## [1] "Associated to"
    ## [1] "with"      "unhealthy" "dietary"   "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "food"     
    ## [1] "exposure"
    ## [1] "Associated to"
    ## [1] "breakfast"     "smoking"       "strengths"     "difficulties" 
    ## [5] "self-efficacy" "absenteeism"   "disabilities"  ")"            
    ## [1] "skipped"
    ## [1] "Associated to"
    ## [1] "who"       "breakfast" "had"       ")"        
    ## [1] "skipped"
    ## [1] "Associated to"
    ## [1] "who"       "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast"
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] "and"        "frequently" "breakfast" 
    ## [1] "time"
    ## [1] "Associated to"
    ## [1] "on"        "screen"    "breakfast" "activity" 
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] "with"      "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "with"      "daily"     "breakfast"
    ## [1] "consumed"
    ## [1] "Associated to"
    ## [1] ","         "alcohol"   "breakfast"
    ## [1] "skipped"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] ","         "and"       "the"       "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "duration"
    ## [1] "Associated to"
    ## [1] "inadequate" "sleep"      "breakfast" 
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] "and"       "breakfast"
    ## [1] "according"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "the"       "breakfast" "boys"     
    ## [1] "related"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "sample"
    ## [1] "Associated to"
    ## [1] ","         "and"       "breakfast" "in"        "the"       "dutch"    
    ## [1] "differences"
    ## [1] "Associated to"
    ## [1] "by"        "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "habit"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] "low"       "breakfast"
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "except"    "for"       "breakfast" "home"      "lunches"   ","        
    ## [1] "patterns"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "eating"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "smoking"  
    ## [1] "use"
    ## [1] "Associated to"
    ## [1] "("          "supplement" "breakfast"  "snacking"   "eating"    
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "eating"   
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] "breakfast"
    ## [1] "intake"
    ## [1] "Associated to"
    ## [1] ","         "including" "breakfast" "viewing"   "sleep"    
    ## [1] "consumption"
    ## [1] "Associated to"
    ## [1] "and"       "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "snacking"  "eating"   
    ## [1] "2/week"
    ## [1] "Associated to"
    ## [1] "breakfast" "≥"        
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast"
    ## [1] "skipping"
    ## [1] "Associated to"
    ## [1] ","         "breakfast" "childhood"

## Of whom sleep is a token id?

``` r
idx<-121
x[idx,]$doc_id
```

    ## [1] "2"

``` r
x[idx,]$sentence_id
```

    ## [1] 8

``` r
idx_downstream_token<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$head_token_id==x[idx,]$token_id)

x[idx_downstream_token,]$token
```

    ## character(0)

``` r
for (idx in idx_breakfast) {
  idx_downstream_token<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$head_token_id==x[idx,]$token_id)
  
  print(x[idx_downstream_token,]$token)
}
```

    ## character(0)
    ## [1] "beliefs"
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "weekly"
    ## character(0)
    ## character(0)
    ## [1] "associated"
    ## character(0)
    ## [1] "ratio"
    ## character(0)
    ## [1] "use"        "days/week"  "associated"
    ## [1] "with"       "fruits"     "vegetables" "milk"       "drinks"    
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "for"   "daily"
    ## [1] ","        "such"     "skipping" "eating"  
    ## character(0)
    ## character(0)
    ## [1] "snack"
    ## [1] "of"         "fruit"      "vegetables"
    ## [1] "of"
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "daily"
    ## character(0)
    ## [1] "of"
    ## [1] "activity"   "behaviours" "obesity"   
    ## [1] "skipping"
    ## character(0)
    ## character(0)
    ## [1] "of"       "symptoms"
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "such"        "skipping"    "odds"        "consumption" "1.35"       
    ## [6] "consuming"  
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] ","     "daily" ","    
    ## character(0)
    ## character(0)
    ## [1] "medication" "diabetes"  
    ## character(0)
    ## [1] "associated"
    ## [1] ","
    ##  [1] "1.30"       "eating"     "inactivity" "1.27"       "viewing"   
    ##  [6] "1.52"       "bedtime"    "1.43"       "duration"   "1.33"      
    ## character(0)
    ## [1] ","       "skipped"
    ## character(0)
    ## [1] "of"   "fish"
    ## character(0)
    ## [1] "and"      "skipping"
    ## character(0)
    ## [1] "to"     "eating"
    ## [1] "of"      "genders" "milk"   
    ## [1] "to"          "consumption" "risks"       "present"    
    ## [1] "use"          "hypoglycemia" "dysesthesia" 
    ## [1] "snacking" "activity" ")"       
    ## character(0)
    ## [1] "in"       "skipping"
    ## character(0)
    ## character(0)
    ## [1] "of"       "day/week"
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] ","
    ## character(0)
    ## [1] "children" "bmi"     
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "="           "snacks"      "="           "consumption" "="

``` r
for (idx in idx_sleep) {
  idx_downstream_token<-which(x$doc_id==x[idx,]$doc_id & x$sentence_id==x[idx,]$sentence_id & x$head_token_id==x[idx,]$token_id)
  
  print(x[idx_downstream_token,]$token)
}
```

    ## [1] "'"        "duration" "people"  
    ## character(0)
    ## character(0)
    ## [1] "suboptimal"
    ## [1] "short"
    ## [1] "non-adequate"
    ## character(0)
    ## character(0)
    ## [1] "that"     "hour/day" "time"    
    ## character(0)
    ## character(0)
    ## [1] "on"       "patterns"
    ## character(0)
    ## [1] "and"
    ## character(0)
    ## [1] ","        "daily"    "duration"
    ## [1] ","         "duration"  "parenting"
    ## [1] ","   "and"
    ## [1] "short/long"
    ## character(0)
    ## character(0)
    ## [1] "and"   "night"
    ## [1] ","        "and"      "duration"
    ## [1] "short"
    ## [1] ","        "duration"
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "("        "duration" ")"       
    ## [1] ","        "and"      "whether"  "duration"
    ## character(0)
    ## [1] ","
    ## [1] "measures" "duration" "."       
    ## [1] ","        "and"      "duration"
    ## character(0)
    ## [1] ","           "consumption" "groups"     
    ## [1] ","               "recommendations" "groups"         
    ## [1] "modes"         "self-reported" "."            
    ## [1] ","        "duration"
    ## [1] "lowest"   "highest"  "children"
    ## character(0)
    ## [1] "insufficient"
    ## character(0)
    ## [1] ","            "and"          "relationship" "conducted"   
    ## [1] ","        "duration"
    ## [1] ","
    ## character(0)
    ## character(0)
    ## [1] ","        "duration"
    ## character(0)
    ## character(0)
    ## [1] "nighttime"
    ## [1] "h"
    ## [1] "students"             "longer"               "associated"          
    ## [4] "muscle-strengthening" "likelihood"           "use"                 
    ## [7] "sweets"               "."                   
    ## character(0)
    ## [1] "and"      "obtained"
    ## [1] "short"
    ## [1] ","        "and"      "longer"   "duration"
    ## [1] "short"
    ## character(0)
    ## [1] ","        "duration"
    ## [1] ","       "quality"
    ## [1] ","        "and"      "duration"
    ## [1] "have" "."   
    ## character(0)
    ## [1] "&"             "v."            "self-reported"
    ## [1] "and"
    ## character(0)
    ## [1] "and"
    ## character(0)
    ## [1] "collected" "."        
    ## [1] "and"
    ## [1] "and"      "duration" "greece"  
    ## [1] "and"
    ## [1] ","        "duration"
    ## [1] "awakening"  "awakening"  "assessment" "snoring"    "decrease"  
    ## [6] "depression" "p<.001"    
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] ","        "duration"
    ## character(0)
    ## [1] "before"     "'"          "television" "h"         
    ## [1] "," "'"
    ## [1] ","        "duration"
    ## [1] "collected" "duration"  "."        
    ## character(0)
    ## [1] ","        "duration"
    ## [1] ","        "duration"
    ## character(0)
    ## character(0)
    ## character(0)
    ## character(0)
    ## [1] "and"      "duration"
    ## [1] "mean"
    ## [1] "short"
    ## [1] "short"
