# straddle

##This is SL_hit, Combo_sl_time, Last_PnL_1
Entry_Time se start
↓
Har minute:
    CE_high + PE_high > Combo_SL ?
        YES → SL_hit = Yes
              Combo_sl_time = SL time
              Last_PnL_1 = CE_PnL + PE_PnL
              STOP
        NO  → last minute PnL track
↓
Agar SL hit nahi hua:
    Combo_sl_time = last candle time
    Last_PnL_1 = day-end PnL


## This is Re_Entry, Re_Entry_Time, Re_Entry_Combo_Price, Re_entry_combo_sl
IF SL_hit != "Yes":
    Re_Entry = NO
    STOP

IF Combo_sl_time >= 15:00:
    Re_Entry = NO
    STOP

IF i > max_reentry:
    Re_Entry = NO
    STOP

IF sl_atm != entry_atm:
    Re_Entry = NO
    STOP

Re_Entry_Time = Combo_sl_time + Time_Gap

CE_close = CE close price at Re_Entry_Time
PE_close = PE close price at Re_Entry_Time

Re_Entry_Combo_Price = CE_close + PE_close

Re_entry_combo_sl = roundoff(
    Re_Entry_Combo_Price × (1 + SL_Percentage / 100)
)











## This is   Re_combo_SL_hit, Re_entry_combo_sl_time, Last_PnL_2
Start from Re_Entry_Time
↓
For each minute >= Re_Entry_Time:
    Combo_High = CE_high + PE_high

    IF Combo_High > Re_entry_combo_sl:
        SL_hit = Yes
        Re_entry_combo_sl_time = current time

        CE_exit_price = CE_close at this minute
        PE_exit_price = PE_close at this minute

        CE_PnL = CE_ReEntry_Price − CE_exit_price
        PE_PnL = PE_ReEntry_Price − PE_exit_price

        Last_PnL_2 = CE_PnL + PE_PnL
        STOP LOOP

    ELSE:
        Track last minute exit prices & PnL

↓
If SL never hit:
    Re_entry_combo_sl_time = last candle time

    CE_exit_price = last CE_close
    PE_exit_price = last PE_close

    CE_PnL = CE_ReEntry_Price − CE_exit_price
    PE_PnL = PE_ReEntry_Price − PE_exit_price

    Last_PnL_2 = CE_PnL + PE_PnL





