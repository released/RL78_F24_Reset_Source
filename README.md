# RL78_F24_Reset_Source
RL78_F24_Reset_Source

udpate @ 2025/10/29

1. use RL78 F24 EVB to test power on source determination , 

refer to below function

```

void get_reset_cause(void)
{
    uint8_t resf = RESF;

    if (POCRES0 == 0)
    {
        POCRES0 = 0x01U;
        printf_tiny("power-on reset\r\n"); 
        return;
    }
    if (resf & BIT7)
    {
        printf_tiny("7)execution of illegal instruction\r\n"); 
        return;      
    }
    if (resf & BIT4)
    {
        printf_tiny("4)watchdog timer (WDT) or clock monitor\r\n"); 
        if (CLKRF == 1)
        {
            printf_tiny("4)clock monitor\r\n"); 
            return;
        }
        else
        {
            printf_tiny("4)watchdog timer (WDT)\r\n"); 
            return;
        }
    }
    if (resf & BIT1)
    {
        printf_tiny("1)illegal-memory access\r\n");    
        return;
    }
    if (resf & BIT0)
    {
        printf_tiny("0)voltage detector (LVD)\r\n");    
        return;
    }
        
    printf_tiny("external reset\r\n");
    return;
}

void checking_reset_source(void)
{
    get_reset_cause();
    RESF   = 0x00U;
    // POCRES = 0x00U;
}

```

2. refer to flow chart as below

![image](https://github.com/released/RL78_F24_Reset_Source/blob/main/flow_chart.jpg)


3. below is log message when trig reset BY power on reset , external reset or illegal-memory reset

![image](https://github.com/released/RL78_F24_Reset_Source/blob/main/pwr_on_log.jpg)

