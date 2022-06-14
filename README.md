## BITCOIN:SATS/BTU ∷ ⨕

```
//@version=3
study("BITCOIN:SATS/BTU ∷ ⨕ ", overlay=true, precision=10, scale=scale.right, max_bars_back=365*4)
shorttitle="⨕"

// Copyright 2022 Randy McMillan
// 
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
// 
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.



// REF: https://www.eia.gov/energyexplained/units-and-calculators/
// Btu content of common energy units (preliminary estimates for 20201)

// 1 barrel (b) of petroleum or related products                      = 42 U.S. gallons
// 1 barrel (42 gallons) of crude oil produced in the United States   = 5,691,000 Btu
// 1 kilowatthour of electricity                                      = 3,412 Btu


// Electricity	1 kilowatthour = 3,412 Btu
// Natural gas	1 cubic foot   = 1,037 Btu
// 1 therm                     = 100,000 Btu
// Motor gasoline	1 gallon   = 120,286 Btu2
// Diesel fuel	1 gallon       = 137,381 Btu3
// Heating oil	1 gallon       = 138,500 Btu4
// Propane	    1 gallon       = 91,452 Btu
// Wood	        1 cord         = 20,000,000 Btu5

// REF: https://www.eia.gov/electricity/state/
//            AVG(cents/kWh)    	  (MW)            (MWh)	          (MWh)
// U.S. Total   	   10.59     1,115,682    4,007,018,595   3,717,674,481

//CENTS_PER_KWH         = 10.59
DOLLARS_PER_KWH         = 0.1059

//MINER_CENTS_PER_KWH   = 3.5
MINER_DOLLARS_PER_KWH   = 0.035

KILOWATTHOUR            = 3412 //BTU

max_bars_back           = 365*4
transparency            = 20
rsi                     = close
interval                = 365
chartResolution         = 0

// CONSTANTS
BTU_PER_BARREL          = 5691000  //
TOTAL_EMISSION          = 21000000 //BTC will ever exist


BTCUSD = input(title="Symbol", type=symbol, defval="BTCUSD")
USOIL  = input(title="Symbol", type=symbol, defval="USOIL")
BTCCAP = input(title="Symbol", type=symbol, defval="CRYPTOCAP:BTC")
BLXUSD = input(title="Symbol", type=symbol, defval="BNC:BLX")

if isdaily
    chartResolution := 24*60*interval
    max_bars_back   := 365*4
    rsi             := rsi(close, max_bars_back)
if isweekly
    chartResolution := 24*60*7*interval
    max_bars_back   := 52*4
    rsi             := rsi(close, max_bars_back)
if ismonthly
    chartResolution := 24*60*30*interval
    max_bars_back   := 12*4
    rsi             := rsi(close, max_bars_back)

USOILCLOSE    = security(USOIL,  period, close)
//plot(USOILCLOSE,title="USOILCLOSE")

BTCUSDCLOSE   = security(BTCUSD, period, close)

// SANITY CHECK
// 100,000,000 SATS = 1 BTC
SATS_PER_USD   = ceil(100000000/BTCUSDCLOSE)
//plot(SATS_PER_USD,title="SATS/USD",color=orange)

// SANITY CHECKS
USD_PER_BARREL  = USOILCLOSE
SATS_PER_BARREL = ceil((USD_PER_BARREL/SATS_PER_USD)*100000000)
//plot(SATS_PER_BARREL,title="SATS/USOIL", color=green)

SAT_PER_BTU     = (SATS_PER_BARREL/BTU_PER_BARREL)
plot(SAT_PER_BTU,title="SAT/BTU", color=white)
```