#NoE// UNIVERSAL RAPID FIRE AND RECOIL BY STRAPCRAFTER78, ANTETHESIS RECOIL MOD CONVERTED TO TITAN ONE
// ALSO FIXED BUG WITH ANTETHESIS WHERE RECOIL LESS THAN ONE WOULDNT WORK
 
// ANY QUESTIONS OR YOUR INTERESTED IN HAVING YOUR OWN CUSTOM SCRIPT MADE FOR YOU
// DONT HESITATE TO MESSAGE ME ON DISCORD STRAPCRAFTER#9465
 
// ################################################################################
// ADJUSTING SETTINGS (XBOX ONE):
// HOLD LT AND USE DPAD TO ADJUST RECOIL (eg if the gun goes up press up dpad until it doesnt go up)
// HOLD A AND USE UP AND DOWN TO ADJUST FIRE RATE (if shots dont register lower the fire rate)
// TURN RAPID FIRE ON AND OFF HOLD MENU THEN PRESS A
// TURN RECOIL ON AND OFF HOLD MENU THEN PRESS Y
// TO SAVE RECOIL AND RAPID FIRE PRESET HOLD L2 AND PRESS MENU
// RAPID FIRE ON = RED LED
// RECOIL ON = GREEN LED 
// RAPID FIRE AND RECOIL ON = BLUE LED
// RAPID FIRE AND RECOIL OFF = NO LED
// RESET RECOIL AND RAPID FIRE TO DEFAULT HOLD LT + RT + LB + RB AND THEN TAP MENU
 
// IF YOU WANT TO FLIP TRIGGERS SET THIS TO TRUE (THIS WILL ALSO INVERT THE TRIGGERS WHEN CHANGING SETTINGS
// FOR EXAMPLE, ADJUSTING RECOIL WILL NOW BE LB AND DPAD NOT LT)
int flip_trig = FALSE;
// ################################################################################
 
int FIRE_RATE = 10;
int RECOIL_V = 30.0;
int RECOIL_H = 0.0;
int RY;
int RX;
int tempV;
int tempX;
int fire_time = 0;
int recoil = TRUE;
int rapid = TRUE;
int first = TRUE;
 
init {
    RECOIL_V = get_pvar(SPVAR_1, -100, 100, 30);
    RECOIL_H = get_pvar(SPVAR_2, -100, 100, 0);
    FIRE_RATE = get_pvar(SPVAR_3, 1, 25, 15);
    recoil = get_pvar(SPVAR_4, FALSE, TRUE, TRUE);
    rapid = get_pvar(SPVAR_5, FALSE, TRUE, TRUE);
}
 
main {
    if (first == TRUE) {
        combo_run(SET_LED);
        first = FALSE;
    }
    if (flip_trig == TRUE) {
        swap(XB1_RT, XB1_RB);
        swap(XB1_LT, XB1_LB);
    }
    if (get_val(XB1_LT)) {
        if (get_val(XB1_RT)) {
            if (recoil == TRUE) {
                combo_run(AntiRECOIL_V);
                combo_run(AntiRECOIL_H);
            }
        }
        if (event_press(XB1_UP) && (RECOIL_V < 100)) {
            RECOIL_V = RECOIL_V + 1;
        }
        if (event_press(XB1_DOWN) && (RECOIL_V > -100)) {
            RECOIL_V = RECOIL_V - 1;
        }
        if (event_press(XB1_LEFT) && (RECOIL_H < 100)) {
            RECOIL_H = RECOIL_H + 1;
        }
        if (event_press(XB1_RIGHT) && (RECOIL_H > -100)) {
            RECOIL_H = RECOIL_H - 1;
        }
        if (event_press(XB1_MENU)) {
            set_pvar(SPVAR_1, RECOIL_V);
            set_pvar(SPVAR_2, RECOIL_H);
            set_pvar(SPVAR_3, FIRE_RATE);
            set_pvar(SPVAR_4, recoil);
            set_pvar(SPVAR_5, rapid);
            combo_run(SAVING_LED);
        }
    }
    if (get_val(XB1_A)) {
        if (event_press(XB1_UP) && (FIRE_RATE < 25)) {
           FIRE_RATE = FIRE_RATE + 1;
        }
        if (event_press(XB1_DOWN) && (FIRE_RATE > 1)) {
            FIRE_RATE = FIRE_RATE - 1;
        }
    }
    if (get_val(XB1_LT) && get_val(XB1_LB) && get_val(XB1_RT) && get_val(XB1_RB) && event_press(XB1_MENU)) {
        set_pvar(SPVAR_1, 30);
        set_pvar(SPVAR_2, 0);
        set_pvar(SPVAR_3, 15);
        RECOIL_V = 30;
        RECOIL_H = 0;
        FIRE_RATE = 15;
    }
    if (get_val(XB1_RT)) {
        if (rapid == TRUE) {
            fire_time = 1000/FIRE_RATE;
            combo_run(rapid_fire);
        }
    }
    if (get_val(XB1_MENU)) {
        if (event_press(XB1_Y)) {
            if (recoil == FALSE) {
                recoil = TRUE;
            } else {
                recoil = FALSE;
            }
            combo_run(SET_LED);
        }
        if (event_press(XB1_A)) {
            if (rapid == FALSE) {
                rapid = TRUE;
            } else {
                rapid = FALSE;
            }
            combo_run(SET_LED);
        }
    }
}
 
 
combo AntiRECOIL_V {
    RY = get_val(XB1_RY); 
    if (RECOIL_V < 0) {
        tempV = RECOIL_V * -1;
    } else {
        tempV = RECOIL_V;
    }
    if (isqrt(RX*RX + RY*RY) <= tempV) {
        if(RY <= tempV){
            set_val(XB1_RY,(RECOIL_V * (100.0 - RY)) / 100.0 + RY);
        }
    }
}
 
combo AntiRECOIL_H {
    RX = get_val(XB1_RX); 
    if (RECOIL_H < 0) {
        tempX = RECOIL_H * -1;
    } else {
        tempX = RECOIL_H;
    }
    if ((isqrt(RX*RX + RY*RY)) <= tempX) {
        if(RY <= tempX){
            set_val(XB1_RX,(RECOIL_H * (100.0 - RX)) / 100.0 + RX);
        }
    }
}
 
combo rapid_fire {
    set_val(XB1_RT, 100);
    wait(30);
    set_val(XB1_RT, 0);
    wait(fire_time);
}
 
combo SAVING_LED {
    set_led(0, 0);
    set_led(1, 1);
    set_led(2, 0);
    set_led(3, 0);
    wait(500):
    set_led(0, 0);
    set_led(1, 0);
    set_led(2, 1);
    set_led(3, 0);
    wait(500);
    set_led(0, 0);
    set_led(1, 1);
    set_led(2, 0);
    set_led(3, 0);
    wait(500):
    set_led(0, 0);
    set_led(1, 0);
    set_led(2, 1);
    set_led(3, 0);
    wait(500);
    combo_run(SET_LED);
}
 
combo SET_LED {
    if ((rapid == TRUE) && (recoil == TRUE)) {
            set_led(0, 1);
            set_led(1, 0);
            set_led(2, 0);
            set_led(3, 0);
        } else if (rapid == TRUE) {
            set_led(0, 0);
            set_led(1, 1);
            set_led(2, 0);
            set_led(3, 0);
        } else if (recoil == TRUE) {
            set_led(0, 0);
            set_led(1, 0);
            set_led(2, 1);
            set_led(3, 0);
        } else if ((recoil == FALSE) && (rapid == FALSE)){
            set_led(0, 0);
            set_led(1, 0);
            set_led(2, 0);
            set_led(3, 0);
    }
}
