# utl-undocumented-options-that-apply-to-procedure-output
Adding variable labels, formats and informats to procedures output tables like sort and transpose  
    Undocumented options that apply to procedure output                                                             
                                                                                                                    
    Adding variable labels, formats and informats to procedures output tables like sort and transpose               
                                                                                                                    
    You get lots of warnings but it works.                                                                          
                                                                                                                    
                                                                                                                    
    *_                   _                                                                                          
    (_)_ __  _ __  _   _| |_                                                                                        
    | | '_ \| '_ \| | | | __|                                                                                       
    | | | | | |_) | |_| | |_                                                                                        
    |_|_| |_| .__/ \__,_|\__|                                                                                       
            |_|                                                                                                     
    ;                                                                                                               
                                                                                                                    
    data have;                                                                                                      
       do year=2012 to 2019;                                                                                        
          do specialty="INTMED","GYNCOL","SURGON","PSYCHI";                                                         
             practitioners=int(1000*uniform(5731));                                                                 
             output;                                                                                                
          end;                                                                                                      
       end;                                                                                                         
    run;quit;                                                                                                       
                                                                                                                    
                                                                                                                    
    /*                                                                                                              
       Variables in Creation Order                                                                                  
                                                                                                                    
    #    Variable         Type    Len                                                                               
                                                                                                                    
    1    YEAR             Num       8                                                                               
    2    SPECIALTY        Char      6                                                                               
    3    PRACTITIONERS    Num       8                                                                               
                                                                                                                    
     WORK.HAVE total obs=32                                                                                         
                                                                                                                    
    Obs    YEAR    SPECIALTY    PRACTITIONERS                                                                       
                                                                                                                    
      1    2012     INTMED            20                                                                            
      2    2012     GYNCOL           578                                                                            
      3    2012     SURGON           393                                                                            
      4    2012     PSYCHI           613                                                                            
                                                                                                                    
      5    2013     INTMED           704                                                                            
      6    2013     GYNCOL           928                                                                            
      7    2013     SURGON           813                                                                            
      8    2013     PSYCHI           613                                                                            
     ...                                                                                                            
     ...                                                                                                            
     29    2019     INTMED           774                                                                            
     30    2019     GYNCOL           663                                                                            
     31    2019     SURGON           240                                                                            
     32    2019     PSYCHI           344                                                                            
    */                                                                                                              
                                                                                                                    
    *            _               _                                                                                  
      ___  _   _| |_ _ __  _   _| |_                                                                                
     / _ \| | | | __| '_ \| | | | __|                                                                               
    | (_) | |_| | |_| |_) | |_| | |_                                                                                
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                               
                    |_|                                                                                             
    ;                                                                                                               
                                                                                                                    
    OUTPUT from ONLY Proc transpose                                                                                 
                                                                                                                    
                                                                                                                    
         Variables in Creation Order                                                                                
                                                                                                                    
    #    Variable    Type    Len    Format      Informat    Label                                                   
                                                                                                                    
    1    YEAR        Num       8    F4.         F4.         Census Year                                             
    2    INVAR       Char     13    $18.        $18.        Var with all Pratitioner types                          
    3    INTMED      Num       8    COMMA10.    F10.        Internal Medicine [INTMED]                              
    4    GYNCOL      Num       8    COMMA10.    F10.        Gynecoligist [GYNCOL]                                   
    5    SURGON      Num       8    COMMA10.    F10.        Surgeon [SURGON]                                        
    6    PSYCHI      Num       8    COMMA10.    F10.        Psychiatrist [PSYCHI]                                   
                                                                                                                    
                                      Internal                                                                      
    Census    Var with all            Medicine    Gynecoligist       Surgeon    Psychiatrist                        
     Year     Pratitioner types       [INTMED]      [GYNCOL]        [SURGON]      [PSYCHI]                          
                                                                                                                    
     2012     PRACTITIONERS                 20            578            393            613                         
     2013     PRACTITIONERS                704            928            813            125                         
     2014     PRACTITIONERS                369            492            514            301                         
     2015     PRACTITIONERS                802             34            703            218                         
     2016     PRACTITIONERS                338            986            839            114                         
     2017     PRACTITIONERS                283            549             73             89                         
     2018     PRACTITIONERS                748            620            457            379                         
     2019     PRACTITIONERS                774            663            240            344                         
                                                                                                                    
    *          _       _   _                                                                                        
     ___  ___ | |_   _| |_(_) ___  _ __                                                                             
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                            
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                           
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                           
                                                                                                                    
    ;                                                                                                               
                                                                                                                    
    * I suspect this also works with many other procedures                                                          
                                                                                                                    
    proc transpose data=have out=havXpo (rename=_name_=InVar);                                                      
    by year;                                                                                                        
    id specialty;                                                                                                   
    var practitioners;                                                                                              
    label                                                                                                           
       year   = "Census Year"                                                                                       
       InVar  = "Var with all Pratitioner types"                                                                    
       INTMED = "Internal Medicine [INTMED]"                                                                        
       GYNCOL = "Gynecoligist [GYNCOL]     "                                                                        
       SURGON = "Surgeon [SURGON]          "                                                                        
       PSYCHI = "Psychiatrist [PSYCHI]     "                                                                        
    ;                                                                                                               
    informat                                                                                                        
       year 4.                                                                                                      
       InVar $18.                                                                                                   
       INTMED                                                                                                       
       GYNCOL                                                                                                       
       SURGON                                                                                                       
       PSYCHI 10.                                                                                                   
    ;                                                                                                               
    format                                                                                                          
       year 4.                                                                                                      
       InVar $18.                                                                                                   
       INTMED                                                                                                       
       GYNCOL                                                                                                       
       SURGON                                                                                                       
       PSYCHI comma10.                                                                                              
    ;                                                                                                               
    run;quit;                                                                                                       
                                                                                                                    
    1071  proc transpose data=have out=havXpo (rename=_name_=InVar);                                                
    1072  by year;                                                                                                  
    1073  id specialty;                                                                                             
    1074  var practitioners;                                                                                        
    1075  label                                                                                                     
    1076     year   = "Census Year"                                                                                 
    1077     InVar  = "Var with all Pratitioner types"                                                              
    1078     INTMED = "Internal Medicine [INTMED]"                                                                  
    WARNING: Variable INVAR not found in data set WORK.HAVE.                                                        
    1079     GYNCOL = "Gynecoligist [GYNCOL]     "                                                                  
    WARNING: Variable INTMED not found in data set WORK.HAVE.                                                       
    1080     SURGON = "Surgeon [SURGON]          "                                                                  
    WARNING: Variable GYNCOL not found in data set WORK.HAVE.                                                       
    1081     PSYCHI = "Psychiatrist [PSYCHI]     "                                                                  
    WARNING: Variable SURGON not found in data set WORK.HAVE.                                                       
    1082  ;                                                                                                         
    WARNING: Variable PSYCHI not found in data set WORK.HAVE.                                                       
    1083  informat                                                                                                  
    1084     year 4.                                                                                                
    1085     InVar $18.                                                                                             
    1086     INTMED                                                                                                 
    WARNING: Variable INVAR not found in data set WORK.HAVE.                                                        
    1087     GYNCOL                                                                                                 
    1088     SURGON                                                                                                 
    1089     PSYCHI 10.                                                                                             
    1090  ;                                                                                                         
    WARNING: Variable INTMED not found in data set WORK.HAVE.                                                       
    WARNING: Variable GYNCOL not found in data set WORK.HAVE.                                                       
    WARNING: Variable SURGON not found in data set WORK.HAVE.                                                       
    WARNING: Variable PSYCHI not found in data set WORK.HAVE.                                                       
    1091  format                                                                                                    
    1092     year 4.                                                                                                
    1093     InVar $18.                                                                                             
    1094     INTMED                                                                                                 
    WARNING: Variable INVAR not found in data set WORK.HAVE.                                                        
    1095     GYNCOL                                                                                                 
    1096     SURGON                                                                                                 
    1097     PSYCHI comma10.                                                                                        
    1098  ;                                                                                                         
    WARNING: Variable INTMED not found in data set WORK.HAVE.                                                       
    WARNING: Variable GYNCOL not found in data set WORK.HAVE.                                                       
    WARNING: Variable SURGON not found in data set WORK.HAVE.                                                       
    WARNING: Variable PSYCHI not found in data set WORK.HAVE.                                                       
    1099  run;                                                                                                      
                                                                                                                    
