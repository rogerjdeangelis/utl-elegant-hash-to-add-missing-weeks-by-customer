# utl-elegant-hash-to-add-missing-weeks-by-customer
Elegant hash to add missing weeks by customer 

    Elegant hash to add missing weeks by customer                                                           
                                                                                                            
    Simple but usefull solution to a common problem.                                                        
                                                                                                            
    github                                                                                                  
    https://tinyurl.com/y599rloq                                                                            
    https://github.com/rogerjdeangelis/utl-elegant-hash-to-add-missing-weeks-by-customer                    
                                                                                                            
    SAS Forum                                                                                               
    https://tinyurl.com/y4ndhsoc                                                                            
    https://communities.sas.com/t5/SAS-Programming/Incrementing-values-of-variable/m-p/570598               
                                                                                                            
    Novinosrin profile                                                                                      
    https://communities.sas.com/t5/user/viewprofilepage/user-id/138205                                      
                                                                                                            
    *_                   _                                                                                  
    (_)_ __  _ __  _   _| |_                                                                                
    | | '_ \| '_ \| | | | __|                                                                               
    | | | | | |_) | |_| | |_                                                                                
    |_|_| |_| .__/ \__,_|\__|                                                                               
            |_|                                                                                             
    ;                                                                                                       
                                                                                                            
    data have;                                                                                              
    input custid week spends;                                                                               
    cards;                                                                                                  
    1 1 4365                                                                                                
    1 2 457                                                                                                 
    1 3 124                                                                                                 
    1 4 35547                                                                                               
    2 5 4678                                                                                                
    2 6 21253                                                                                               
    3 7 32534                                                                                               
    3 1 23423                                                                                               
    3 2 23567                                                                                               
    ;                                                                                                       
    run;                                                                                                    
                                                                                                            
    WORK.HAVE total obs=9                                                                                   
                                                                                                            
     CUSTID    WEEK    SPENDS                                                                               
                                                                                                            
        1        1       4365                                                                               
        1        2        457                                                                               
        1        3        124                                                                               
        1        4      35547                                                                               
        2        5       4678                                                                               
        2        6      21253                                                                               
        3        7      32534                                                                               
        3        1      23423                                                                               
        3        2      23567                                                                               
                                                                                                            
    *            _               _                                                                          
      ___  _   _| |_ _ __  _   _| |_                                                                        
     / _ \| | | | __| '_ \| | | | __|                                                                       
    | (_) | |_| | |_| |_) | |_| | |_                                                                        
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                       
                    |_|                                                                                     
    ;                                                                                                       
                                                                                                            
    Up to 40 obs WORK.WANT total obs=21                                                                     
                                       | RULES                                                              
                                       |                                                                    
    Obs    CUSTID    WEEK    SPENDS    |                                                                    
                                       |                                                                    
      1       1        1       4365    |                                                                    
      2       1        2        457    |                                                                    
      3       1        3        124    |                                                                    
      4       1        4      35547    |                                                                    
      5       1        5          .    | Fill in missing weeks                                              
      6       1        6          .    | with missings                                                      
      7       1        7          .    |                                                                    
                                                                                                            
      8       2        1          .    | Before fill                                                        
      9       2        2          .    |                                                                    
     10       2        3          .    |                                                                    
     11       2        4          .    |                                                                    
     12       2        5       4678    |                                                                    
     13       2        6      21253    |                                                                    
     14       2        7          .    |                                                                    
                                                                                                            
     15       3        1      23423    |                                                                    
     16       3        2      23567    |                                                                    
     17       3        3          .    | Middle fill                                                        
     18       3        4          .    |                                                                    
     19       3        5          .    |                                                                    
     20       3        6          .    |                                                                    
     21       3        7      32534    |                                                                    
                                                                                                            
                                                                                                            
    *                                                                                                       
     _ __  _ __ ___   ___ ___  ___ ___                                                                      
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                     
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                     
    | .__/|_|  \___/ \___\___||___/___/                                                                     
    |_|                                                                                                     
    ;                                                                                                       
                                                                                                            
    %let num_of_weeks=7;                                                                                    
    data want ;                                                                                             
    if _n_=1 then do;                                                                                       
          if 0 then set have;                                                                               
       dcl hash H (dataset: "have") ;                                                                       
       h.definekey  ("custid","week") ;                                                                     
       h.definedata ("spends") ;                                                                            
       h.definedone () ;                                                                                    
       end;                                                                                                 
    set have(keep=custid);                                                                                  
    by custid;                                                                                              
    if first.custid;                                                                                        
    do week=1 to &num_of_weeks;                                                                             
    if h.find() ne 0 then call missing(spends);                                                             
    output;                                                                                                 
    end;                                                                                                    
    run;                                                                                                    
                                                                                                            
                                                                                                            
                                                                                                            
