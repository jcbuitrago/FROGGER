/*######################################################################
//#	G0B1T: HDL EXAMPLES. 2018.
//######################################################################
//# Copyright (C) 2018. F.E.Segura-Quijano (FES) fsegura@uniandes.edu.co
//# 
//# This program is free software: you can redistribute it and/or modify
//# it under the terms of the GNU General Public License as published by
//# the Free Software Foundation, version 3 of the License.
//#
//# This program is distributed in the hope that it will be useful,
//# but WITHOUT ANY WARRANTY; without even the implied warranty of
//# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
//# GNU General Public License for more details.
//#
//# You should have received a copy of the GNU General Public License
//# along with this program.  If not, see <http://www.gnu.org/licenses/>
//####################################################################*/
//=======================================================
//  MODULE Definition
//=======================================================
module BB_SYSTEM (
//////////// OUTPUTS //////////
	BB_SYSTEM_display_OutBUS,
	BB_SYSTEM_max7219DIN_Out,
	BB_SYSTEM_max7219NCS_Out,
	BB_SYSTEM_max7219CLK_Out,

	BB_SYSTEM_startButton_Out, 
	BB_SYSTEM_upButton_Out,
	BB_SYSTEM_downButton_Out,
	BB_SYSTEM_leftButton_Out,
	BB_SYSTEM_rightButton_Out,
	BB_SYSTEM_TEST0,
	BB_SYSTEM_TEST1,
	BB_SYSTEM_TEST2,

//////////// INPUTS //////////
	BB_SYSTEM_CLOCK_50,
	BB_SYSTEM_RESET_InHigh,
	BB_SYSTEM_startButton_InLow, 
	BB_SYSTEM_upButton_InLow,
	BB_SYSTEM_downButton_InLow,
	BB_SYSTEM_leftButton_InLow,
	BB_SYSTEM_rightButton_InLow
);
//=======================================================
//  PARAMETER declarations
//=======================================================
 parameter DATAWIDTH_BUS = 8;
 parameter PRESCALER_DATAWIDTH = 22;
 parameter DISPLAY_DATAWIDTH = 12;
 parameter BINOMIAL_BUS = 2;
 
 parameter DATA_FIXED_INITREGFROGGER_7 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_6 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_5 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_4 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_3 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_2 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_1 = 8'b00000000;
 parameter DATA_FIXED_INITREGFROGGER_0 = 8'b00000001;
 
 parameter DATA_FIXED_INITREGCARS_7 = 8'b00000000;
 parameter DATA_FIXED_INITREGCARS_6 = 8'b00000000;
 parameter DATA_FIXED_INITREGCARS_5 = 8'b00000011;
 parameter DATA_FIXED_INITREGCARS_4 = 8'b00000000;
 parameter DATA_FIXED_INITREGCARS_3 = 8'b00011000;
 parameter DATA_FIXED_INITREGCARS_2 = 8'b00000000;
 parameter DATA_FIXED_INITREGCARS_1 = 8'b01100000;
 parameter DATA_FIXED_INITREGCARS_0 = 8'b00000000;
 
 parameter DATA_FIXED_INITREGHOUSES_7 = 8'b11011011;
 
//=======================================================
//  PORT declarations
//=======================================================
output		[DISPLAY_DATAWIDTH-1:0] BB_SYSTEM_display_OutBUS;

output		BB_SYSTEM_max7219DIN_Out;
output		BB_SYSTEM_max7219NCS_Out;
output		BB_SYSTEM_max7219CLK_Out;

output 		BB_SYSTEM_startButton_Out;
output 		BB_SYSTEM_upButton_Out;
output 		BB_SYSTEM_downButton_Out;
output 		BB_SYSTEM_leftButton_Out;
output 		BB_SYSTEM_rightButton_Out;
output 		BB_SYSTEM_TEST0;
output 		BB_SYSTEM_TEST1;
output 		BB_SYSTEM_TEST2;

input		BB_SYSTEM_CLOCK_50;
input		BB_SYSTEM_RESET_InHigh;
input		BB_SYSTEM_startButton_InLow;
input		BB_SYSTEM_upButton_InLow;
input		BB_SYSTEM_downButton_InLow;
input		BB_SYSTEM_leftButton_InLow;
input		BB_SYSTEM_rightButton_InLow;
//=======================================================
//  REG/WIRE declarations
//=======================================================

//FROGGER
wire	STATEMACHINEFROGGER_clear_cwire;
wire	STATEMACHINEFROGGER_load0_cwire;
wire	STATEMACHINEFROGGER_load1_cwire;
wire	[1:0] STATEMACHINEFROGGER_shiftselection_cwire;

wire 	BB_SYSTEM_startButton_InLow_cwire;
wire 	BB_SYSTEM_upButton_InLow_cwire;
wire 	BB_SYSTEM_downButton_InLow_cwire;
wire 	BB_SYSTEM_leftButton_InLow_cwire;
wire 	BB_SYSTEM_rightButton_InLow_cwire;

wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out;
wire [DATAWIDTH_BUS-1:0] RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out;

//CARS
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data7_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data6_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data5_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data4_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data3_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data2_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data1_Out;
wire [DATAWIDTH_BUS-1:0] RegCARSTYPE_2_CARSMATRIX_data0_Out;

wire [PRESCALER_DATAWIDTH-1:0] upSPEEDCOUNTER_data_BUS_wire;
wire SPEEDCOMPARATOR_2_STATEMACHINECARS_T0_cwire;
wire STATEMACHINECARS_clear_cwire;
wire STATEMACHINECARS_load_cwire;
wire [1:0] STATEMACHINECARS_shiftselection_R_cwire;
wire [1:0] STATEMACHINECARS_shiftselection_L_cwire;
wire STATEMACHINECARS_upcount_cwire;

//HOUSES
wire [DATAWIDTH_BUS-1:0] RegHOUSESTYPE_2_HOUSESMATRIX_data7_Out;

//BOTTOMSIDE COMPARATOR
wire BOTTOMSIDECOMPARATOR_2_STATEMACHINECARS_bottomside_cwire;

// GAME
wire [DATAWIDTH_BUS-1:0] regGAME_data7_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data6_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data5_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data4_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data3_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data2_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data1_wire;
wire [DATAWIDTH_BUS-1:0] regGAME_data0_wire;

// AND
wire [DATAWIDTH_BUS-1:0] regAND_data7_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data6_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data5_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data4_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data3_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data2_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data1_wire;
wire [DATAWIDTH_BUS-1:0] regAND_data0_wire;
wire [DATAWIDTH_BUS-1:0] regDEAD_data_wire;

// LEVEL
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars1_wire;
wire  [DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars2_wire;
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars3_wire;
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars4_wire;
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars5_wire;
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regcars6_wire;
wire	[DATAWIDTH_BUS-1:0]LEVELSTYPE_regHouse_wire;
wire	[1:0]LEVELCOUNTER_data_wire;

// WIN
wire WINCOMPARATOR_win_OutLow_wire;
wire WINCOMPARATOR_winLevel_OutLow_wire;
wire WINCOMPARATOR_Reset_wire;
wire [DATAWIDTH_BUS-1:0]CC_WINCOMPARATOR_data_wire;
// DEAD
wire DEADCOUNTER_deadstate_wire;

wire 	[7:0] data_max;
wire 	[2:0] add;



wire [DATAWIDTH_BUS-1:0] upCOUNTER_2_BIN2BCD1_data_BUS_wire;
wire [DISPLAY_DATAWIDTH-1:0] BIN2BCD1_2_SEVENSEG1_data_BUS_wire;
//wire STATEMACHINE_upcount_cwire;


//=======================================================
//  Structural coding
//=======================================================

//######################################################################
//#	INPUTS
//######################################################################
SC_DEBOUNCE1 SC_DEBOUNCE1_u0 (
// port map - connection between master ports and signals/registers   
	.SC_DEBOUNCE1_button_Out(BB_SYSTEM_startButton_InLow_cwire),
	.SC_DEBOUNCE1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_DEBOUNCE1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_DEBOUNCE1_button_In(~BB_SYSTEM_startButton_InLow)
);
SC_DEBOUNCE1 SC_DEBOUNCE1_u1 (
// port map - connection between master ports and signals/registers   
	.SC_DEBOUNCE1_button_Out(BB_SYSTEM_upButton_InLow_cwire),
	.SC_DEBOUNCE1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_DEBOUNCE1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_DEBOUNCE1_button_In(~BB_SYSTEM_upButton_InLow)
);
SC_DEBOUNCE1 SC_DEBOUNCE1_u2 (
// port map - connection between master ports and signals/registers   
	.SC_DEBOUNCE1_button_Out(BB_SYSTEM_downButton_InLow_cwire),
	.SC_DEBOUNCE1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_DEBOUNCE1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_DEBOUNCE1_button_In(~BB_SYSTEM_downButton_InLow)
);
SC_DEBOUNCE1 SC_DEBOUNCE1_u3 (
// port map - connection between master ports and signals/registers   
	.SC_DEBOUNCE1_button_Out(BB_SYSTEM_leftButton_InLow_cwire),
	.SC_DEBOUNCE1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_DEBOUNCE1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_DEBOUNCE1_button_In(~BB_SYSTEM_leftButton_InLow)
);
SC_DEBOUNCE1 SC_DEBOUNCE1_u4 (
// port map - connection between master ports and signals/registers   
	.SC_DEBOUNCE1_button_Out(BB_SYSTEM_rightButton_InLow_cwire),
	.SC_DEBOUNCE1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_DEBOUNCE1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_DEBOUNCE1_button_In(~BB_SYSTEM_rightButton_InLow)
);

//######################################################################
//#	FROGGER
//######################################################################
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_7)) SC_RegFROGGERTYPE_u7 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_6)) SC_RegFROGGERTYPE_u6 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_5)) SC_RegFROGGERTYPE_u5 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_4)) SC_RegFROGGERTYPE_u4 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_3)) SC_RegFROGGERTYPE_u3 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_2)) SC_RegFROGGERTYPE_u2 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_1)) SC_RegFROGGERTYPE_u1 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out)
);
SC_RegFROGGERTYPE #(.RegFROGGERTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGFROGGER(DATA_FIXED_INITREGFROGGER_0)) SC_RegFROGGERTYPE_u0 (
// port map - connection between master ports and signals/registers   
	.SC_RegFROGGERTYPE_data_OutBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out),
	.SC_RegFROGGERTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegFROGGERTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegFROGGERTYPE_clear_InLow(STATEMACHINEFROGGER_clear_cwire),
	.SC_RegFROGGERTYPE_load0_InLow(STATEMACHINEFROGGER_load0_cwire),
	.SC_RegFROGGERTYPE_load1_InLow(STATEMACHINEFROGGER_load1_cwire),
	.SC_RegFROGGERTYPE_shiftselection_In(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_RegFROGGERTYPE_data0_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out),
	.SC_RegFROGGERTYPE_data1_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out)
);

SC_STATEMACHINEFROGGER SC_STATEMACHINEFROGGER_u0 (
// port map - connection between master ports and signals/registers   
	.SC_STATEMACHINEFROGGER_clear_OutLow(STATEMACHINEFROGGER_clear_cwire), 
	.SC_STATEMACHINEFROGGER_load0_OutLow(STATEMACHINEFROGGER_load0_cwire), 
	.SC_STATEMACHINEFROGGER_load1_OutLow(STATEMACHINEFROGGER_load1_cwire), 
	.SC_STATEMACHINEFROGGER_shiftselection_Out(STATEMACHINEFROGGER_shiftselection_cwire),
	.SC_STATEMACHINEFROGGER_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_STATEMACHINEFROGGER_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_STATEMACHINEFROGGER_startButton_InLow(BB_SYSTEM_startButton_InLow_cwire), 
	.SC_STATEMACHINEFROGGER_upButton_InLow(BB_SYSTEM_upButton_InLow_cwire), 
	.SC_STATEMACHINEFROGGER_downButton_InLow(BB_SYSTEM_downButton_InLow_cwire), 
	.SC_STATEMACHINEFROGGER_leftButton_InLow(BB_SYSTEM_leftButton_InLow_cwire), 
	.SC_STATEMACHINEFROGGER_rightButton_InLow(BB_SYSTEM_rightButton_InLow_cwire), 
	.SC_STATEMACHINEFROGGER_bottomsidecomparator_InLow(BOTTOMSIDECOMPARATOR_2_STATEMACHINECARS_bottomside_cwire),
	.SC_STATEMACHINEFROGGER_win_InLow(WINCOMPARATOR_win_OutLow_wire),
	.SC_STATEMACHINEFROGGER_dead_InLow(DEADCOUNTER_deadstate_wire)
);

//######################################################################
//#	CARS
//######################################################################
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_7)) SC_RegCARSTYPE_u7 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data7_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_R_cwire),
	.SC_RegCARSTYPE_data_InBUS(DATA_FIXED_INITREGCARS_0)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_6)) SC_RegCARSTYPE_u6 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data6_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_L_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars6_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_5)) SC_RegCARSTYPE_u5 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data5_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_R_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars5_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_4)) SC_RegCARSTYPE_u4 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data4_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_L_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars4_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_3)) SC_RegCARSTYPE_u3 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data3_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_R_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars3_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_2)) SC_RegCARSTYPE_u2 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data2_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_L_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars2_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_1)) SC_RegCARSTYPE_u1 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data1_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_R_cwire),
	.SC_RegCARSTYPE_data_InBUS(LEVELSTYPE_regcars1_wire)
);
SC_RegCARSTYPE #(.RegCARSTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGCARS(DATA_FIXED_INITREGCARS_0)) SC_RegCARSTYPE_u0 (
// port map - connection between master ports and signals/registers   
	.SC_RegCARSTYPE_data_OutBUS(RegCARSTYPE_2_CARSMATRIX_data0_Out),
	.SC_RegCARSTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegCARSTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegCARSTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegCARSTYPE_load_InLow(STATEMACHINECARS_load_cwire),
	.SC_RegCARSTYPE_shiftselection_In(STATEMACHINECARS_shiftselection_L_cwire),
	.SC_RegCARSTYPE_data_InBUS(DATA_FIXED_INITREGCARS_0)
);
SC_STATEMACHINECARS SC_STATEMACHINECARS_u0 (
// port map - connection between master ports and signals/registers   
	.SC_STATEMACHINECARS_clear_OutLow(STATEMACHINECARS_clear_cwire), 
	.SC_STATEMACHINECARS_load_OutLow(STATEMACHINECARS_load_cwire), 
	.SC_STATEMACHINECARS_shiftselection_R_Out(STATEMACHINECARS_shiftselection_R_cwire),
	.SC_STATEMACHINECARS_shiftselection_L_Out(STATEMACHINECARS_shiftselection_L_cwire),
	.SC_STATEMACHINECARS_upcount_out(STATEMACHINECARS_upcount_cwire),
	.SC_STATEMACHINECARS_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_STATEMACHINECARS_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_STATEMACHINECARS_startButton_InLow(BB_SYSTEM_startButton_InLow_cwire),
	.SC_STATEMACHINECARS_T0_InLow(SPEEDCOMPARATOR_2_STATEMACHINECARS_T0_cwire),
	.SC_STATEMACHINECARS_win_InLow(WINCOMPARATOR_winLevel_OutLow_wire)
);

//#SPEED
SC_upSPEEDCOUNTER #(.upSPEEDCOUNTER_DATAWIDTH(PRESCALER_DATAWIDTH)) SC_upSPEEDCOUNTER_u0 (
// port map - connection between master ports and signals/registers   
	.SC_upSPEEDCOUNTER_data_OutBUS(upSPEEDCOUNTER_data_BUS_wire),
	.SC_upSPEEDCOUNTER_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_upSPEEDCOUNTER_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_upSPEEDCOUNTER_upcount_InLow(STATEMACHINECARS_upcount_cwire)
);

CC_SPEEDCOMPARATOR #(.SPEEDCOMPARATOR_DATAWIDTH(PRESCALER_DATAWIDTH)) CC_SPEEDCOMPARATOR_u0 (
	.CC_SPEEDCOMPARATOR_T0_OutLow(SPEEDCOMPARATOR_2_STATEMACHINECARS_T0_cwire),
	.CC_SPEEDCOMPARATOR_data_InBUS(upSPEEDCOUNTER_data_BUS_wire)
);

//######################################################################
//#	HOUSES
//######################################################################
SC_RegHOUSESTYPE #(.RegHOUSESTYPE_DATAWIDTH(DATAWIDTH_BUS), .DATA_FIXED_INITREGHOUSES(DATA_FIXED_INITREGHOUSES_7)) SC_RegHOUSESTYPE_u1 (
// port map - connection between master ports and signals/registers   
	.SC_RegHOUSESTYPE_data_OutBUS(RegHOUSESTYPE_2_HOUSESMATRIX_data7_Out),
	.SC_RegHOUSESTYPE_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_RegHOUSESTYPE_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_RegHOUSESTYPE_clear_InLow(STATEMACHINECARS_clear_cwire),	
	.SC_RegHOUSESTYPE_load_InLow(WINCOMPARATOR_win_OutLow_wire),
	.SC_RegHOUSESTYPE_shiftselection_In(2'b00),
	.SC_RegHOUSESTYPE_data_InBUS(CC_WINCOMPARATOR_data_wire)
);


CC_WINCOMPARATOR #(.WINCOMPARATOR_DATAWIDTH(DATAWIDTH_BUS)) CC_WINCOMPARATOR_u0 (
	.CC_WINCOMPARATOR_win_OutLow(WINCOMPARATOR_win_OutLow_wire),
	.CC_WINCOMPARATOR_winLevel_OutLow(WINCOMPARATOR_winLevel_OutLow_wire),
	.CC_WINCOMPARATOR_data_OutBUS(CC_WINCOMPARATOR_data_wire),
	.SC_WINCOMPARATOR_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_WINCOMPARATOR_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.CC_WINCOMPARATOR_data_InBUS(regGAME_data7_wire),
	.CC_WINCOMPARATOR_Regster_InBUS(RegHOUSESTYPE_2_HOUSESMATRIX_data7_Out)
);

CC_LEVELSTYPE #(.LEVELSTYPE_DATAWIDTH(DATAWIDTH_BUS)) CC_LEVELSTYPE_u0 (
	.CC_LEVELSTYPE_regcars1_OutBUS(LEVELSTYPE_regcars1_wire),
	.CC_LEVELSTYPE_regcars2_OutBUS(LEVELSTYPE_regcars2_wire),
	.CC_LEVELSTYPE_regcars3_OutBUS(LEVELSTYPE_regcars3_wire),
	.CC_LEVELSTYPE_regcars4_OutBUS(LEVELSTYPE_regcars4_wire),
	.CC_LEVELSTYPE_regcars5_OutBUS(LEVELSTYPE_regcars5_wire),
	.CC_LEVELSTYPE_regcars6_OutBUS(LEVELSTYPE_regcars6_wire),
	.CC_LEVELSTYPE_regHouse_OutBUS(LEVELSTYPE_regHouse_wire),
	.CC_LEVELSTYPE_levelcount_InBUS(LEVELCOUNTER_data_wire)
);

SC_LEVELCOUNTER #(.LEVELCOUNTER_DATAWIDTH(BINOMIAL_BUS)) SC__LEVELCOUNTER_u0(
	.SC_LEVELCOUNTER_data_OutBUS(LEVELCOUNTER_data_wire),
	.SC_LEVELCOUNTER_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_LEVELCOUNTER_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_LEVELCOUNTER_upcount_InLow(WINCOMPARATOR_winLevel_OutLow_wire)
);

CC_DEADCOUNTER #(.DEADCOUNTER_DATAWIDTH(DATAWIDTH_BUS)) CC_DEADCOUNTER_u0(
	.CC_DEADCOUNTER_deadstate_OutLow(DEADCOUNTER_deadstate_wire),
	.CC_DEADCOUNTER_data_InBUS(regDEAD_data_wire)
);
//######################################################################
//#	COMPARATOR END OF MATRIX (BOTTON SIDE)
//######################################################################
CC_BOTTOMSIDECOMPARATOR #(.BOTTOMSIDECOMPARATOR_DATAWIDTH(DATAWIDTH_BUS)) CC_BOTTOMSIDECOMPARATOR_u0 (
	.CC_BOTTOMSIDECOMPARATOR_bottomside_OutLow(BOTTOMSIDECOMPARATOR_2_STATEMACHINECARS_bottomside_cwire),
	.CC_BOTTOMSIDECOMPARATOR_data_InBUS(RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out)
);

//######################################################################
//#	TO LED MATRIZ: VISUALIZATION
//######################################################################
assign regGAME_data0_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out | RegCARSTYPE_2_CARSMATRIX_data0_Out;
assign regGAME_data1_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out | RegCARSTYPE_2_CARSMATRIX_data1_Out;
assign regGAME_data2_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out | RegCARSTYPE_2_CARSMATRIX_data2_Out;
assign regGAME_data3_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out | RegCARSTYPE_2_CARSMATRIX_data3_Out;
assign regGAME_data4_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out | RegCARSTYPE_2_CARSMATRIX_data4_Out;
assign regGAME_data5_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out | RegCARSTYPE_2_CARSMATRIX_data5_Out;
assign regGAME_data6_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out | RegCARSTYPE_2_CARSMATRIX_data6_Out;
assign regGAME_data7_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out | RegCARSTYPE_2_CARSMATRIX_data7_Out | RegHOUSESTYPE_2_HOUSESMATRIX_data7_Out;

assign regAND_data0_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data0_Out & RegCARSTYPE_2_CARSMATRIX_data0_Out;
assign regAND_data1_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data1_Out & RegCARSTYPE_2_CARSMATRIX_data1_Out;
assign regAND_data2_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data2_Out & RegCARSTYPE_2_CARSMATRIX_data2_Out;
assign regAND_data3_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data3_Out & RegCARSTYPE_2_CARSMATRIX_data3_Out;
assign regAND_data4_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data4_Out & RegCARSTYPE_2_CARSMATRIX_data4_Out;
assign regAND_data5_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data5_Out & RegCARSTYPE_2_CARSMATRIX_data5_Out;
assign regAND_data6_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data6_Out & RegCARSTYPE_2_CARSMATRIX_data6_Out;
assign regAND_data7_wire = RegFROGGERTYPE_2_FROGGERMATRIX_data7_Out & RegHOUSESTYPE_2_HOUSESMATRIX_data7_Out;

assign regDEAD_data_wire = regAND_data0_wire | regAND_data1_wire | regAND_data2_wire | regAND_data3_wire | regAND_data4_wire | regAND_data5_wire | regAND_data6_wire | regAND_data7_wire;


assign data_max =(add==3'b000)?{regGAME_data0_wire[7],regGAME_data1_wire[7],regGAME_data2_wire[7],regGAME_data3_wire[7],regGAME_data4_wire[7],regGAME_data5_wire[7],regGAME_data6_wire[7],regGAME_data7_wire[7]}:
	       (add==3'b001)?{regGAME_data0_wire[6],regGAME_data1_wire[6],regGAME_data2_wire[6],regGAME_data3_wire[6],regGAME_data4_wire[6],regGAME_data5_wire[6],regGAME_data6_wire[6],regGAME_data7_wire[6]}:
	       (add==3'b010)?{regGAME_data0_wire[5],regGAME_data1_wire[5],regGAME_data2_wire[5],regGAME_data3_wire[5],regGAME_data4_wire[5],regGAME_data5_wire[5],regGAME_data6_wire[5],regGAME_data7_wire[5]}:
	       (add==3'b011)?{regGAME_data0_wire[4],regGAME_data1_wire[4],regGAME_data2_wire[4],regGAME_data3_wire[4],regGAME_data4_wire[4],regGAME_data5_wire[4],regGAME_data6_wire[4],regGAME_data7_wire[4]}:
	       (add==3'b100)?{regGAME_data0_wire[3],regGAME_data1_wire[3],regGAME_data2_wire[3],regGAME_data3_wire[3],regGAME_data4_wire[3],regGAME_data5_wire[3],regGAME_data6_wire[3],regGAME_data7_wire[3]}:
	       (add==3'b101)?{regGAME_data0_wire[2],regGAME_data1_wire[2],regGAME_data2_wire[2],regGAME_data3_wire[2],regGAME_data4_wire[2],regGAME_data5_wire[2],regGAME_data6_wire[2],regGAME_data7_wire[2]}:
	       (add==3'b110)?{regGAME_data0_wire[1],regGAME_data1_wire[1],regGAME_data2_wire[1],regGAME_data3_wire[1],regGAME_data4_wire[1],regGAME_data5_wire[1],regGAME_data6_wire[1],regGAME_data7_wire[1]}:
						{regGAME_data0_wire[0],regGAME_data1_wire[0],regGAME_data2_wire[0],regGAME_data3_wire[0],regGAME_data4_wire[0],regGAME_data5_wire[0],regGAME_data6_wire[0],regGAME_data7_wire[0]};
									 
			//{regGAME_data7_wire[7],regGAME_data6_wire[7],regGAME_data5_wire[7],regGAME_data4_wire[7],regGAME_data3_wire[7],regGAME_data2_wire[7],regGAME_data1_wire[7],regGAME_data0_wire[7]}
//~ assign data_max =(add==3'b000)?regGAME_data0_wire:
	       //~ (add==3'b001)?regGAME_data1_wire:
	       //~ (add==3'b010)?regGAME_data2_wire:
	       //~ (add==3'b011)?regGAME_data3_wire:
	       //~ (add==3'b100)?regGAME_data4_wire:
	       //~ (add==3'b101)?regGAME_data5_wire:
	       //~ (add==3'b110)?regGAME_data6_wire:
						//~ regGAME_data7_wire;
									 
matrix_ctrl matrix_ctrl_unit_0( 
.max7219_din(BB_SYSTEM_max7219DIN_Out),//max7219_din 
.max7219_ncs(BB_SYSTEM_max7219NCS_Out),//max7219_ncs 
.max7219_clk(BB_SYSTEM_max7219CLK_Out),//max7219_clk
.disp_data(data_max), 
.disp_addr(add),
.intensity(4'hA),
.clk(BB_SYSTEM_CLOCK_50),
.reset(BB_SYSTEM_RESET_InHigh) //~lowRst_System
 ); 
 
//~ imagen img1(
//~ .act_add(add), 
//~ .max_in(data_max) );

//~ SC_CTRLMATRIX SC_CTRLMATRIX_u0( 
//~ .SC_CTRLMATRIX_max7219DIN_Out(BB_SYSTEM_max7219DIN_Out),	//max7219_din 
//~ .SC_CTRLMATRIX_max7219NCS_out(BB_SYSTEM_max7219NCS_Out),	//max7219_ncs 
//~ .SC_CTRLMATRIX_max7219CLK_Out(BB_SYSTEM_max7219CLK_Out),	//max7219_clk
//~ .SC_CTRLMATRIX_dispdata(data_max), 
//~ .SC_CTRLMATRIX_dispaddr(add),
//~ .SC_CTRLMATRIX_intensity(4'hA),
//~ .SC_CTRLMATRIX_CLOCK_50(BB_SYSTEM_CLOCK_50),
//~ .SC_CTRLMATRIX_RESET_InHigh(~BB_SYSTEM_RESET_InHigh) 		//~lowRst_System
 //~ ); 
 
//~ SC_IMAGE SC_IMAGE_u0(
//~ .SC_IMAGE_actadd(add), 
//~ .SC_IMAGE_maxin(data_max) );

//######################################################################
//#	TO TEST
//######################################################################

assign BB_SYSTEM_startButton_Out = BB_SYSTEM_startButton_InLow_cwire;
assign BB_SYSTEM_upButton_Out = BB_SYSTEM_upButton_InLow_cwire;
assign BB_SYSTEM_downButton_Out = BB_SYSTEM_downButton_InLow_cwire;
assign BB_SYSTEM_leftButton_Out = BB_SYSTEM_leftButton_InLow_cwire;
assign BB_SYSTEM_rightButton_Out = BB_SYSTEM_rightButton_InLow_cwire;

//assign BB_SYSTEM_TEST0 = ^((^regGAME_data0_wire) ^ (^regGAME_data1_wire) ^ (^regGAME_data2_wire) ^ (^regGAME_data3_wire) ^ (^regGAME_data4_wire) ^ (^regGAME_data5_wire) ^ (^regGAME_data6_wire) ^ (^regGAME_data7_wire));
//assign BB_SYSTEM_TEST1 = ^((^regGAME_data0_wire) ^ (^regGAME_data1_wire) ^ (^regGAME_data2_wire) ^ (^regGAME_data3_wire) ^ (^regGAME_data4_wire) ^ (^regGAME_data5_wire) ^ (^regGAME_data6_wire) ^ (^regGAME_data7_wire));
//assign BB_SYSTEM_TEST2 = ^((^regGAME_data0_wire) ^ (^regGAME_data1_wire) ^ (^regGAME_data2_wire) ^ (^regGAME_data3_wire) ^ (^regGAME_data4_wire) ^ (^regGAME_data5_wire) ^ (^regGAME_data6_wire) ^ (^regGAME_data7_wire));

assign BB_SYSTEM_TEST0 = LEVELCOUNTER_data_wire[0];
assign BB_SYSTEM_TEST1 = LEVELCOUNTER_data_wire[1];

CC_BIN2BCD1 CC_BIN2BCD1_u0 (
// port map - connection between master ports and signals/registers   
	.CC_BIN2BCD_bcd_OutBUS(BIN2BCD1_2_SEVENSEG1_data_BUS_wire),
	.CC_BIN2BCD_bin_InBUS(upCOUNTER_2_BIN2BCD1_data_BUS_wire)
);

CC_SEVENSEG1 CC_SEVENSEG1_u0 (
// port map - connection between master ports and signals/registers   
	.CC_SEVENSEG1_an(BB_SYSTEM_display_OutBUS[11:8]),
	.CC_SEVENSEG1_a(BB_SYSTEM_display_OutBUS[0]),
	.CC_SEVENSEG1_b(BB_SYSTEM_display_OutBUS[1]),
	.CC_SEVENSEG1_c(BB_SYSTEM_display_OutBUS[2]),
	.CC_SEVENSEG1_d(BB_SYSTEM_display_OutBUS[3]),
	.CC_SEVENSEG1_e(BB_SYSTEM_display_OutBUS[4]),
	.CC_SEVENSEG1_f(BB_SYSTEM_display_OutBUS[5]),
	.CC_SEVENSEG1_g(BB_SYSTEM_display_OutBUS[6]),
	.CC_SEVENSEG1_dp(BB_SYSTEM_display_OutBUS[7]),
	.CC_SEVENSEG1_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.CC_SEVENSEG1_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.CC_SEVENSEG1_in0(BIN2BCD1_2_SEVENSEG1_data_BUS_wire[3:0]),
	.CC_SEVENSEG1_in1(BIN2BCD1_2_SEVENSEG1_data_BUS_wire[7:4]),
	.CC_SEVENSEG1_in2(BIN2BCD1_2_SEVENSEG1_data_BUS_wire[11:8]),
	.CC_SEVENSEG1_in3(BIN2BCD1_2_SEVENSEG1_data_BUS_wire[11:8])
);

SC_upCOUNTER #(.upCOUNTER_DATAWIDTH(DATAWIDTH_BUS)) SC_upCOUNTER_u0 (
// port map - connection between master ports and signals/registers   
	.SC_upCOUNTER_data_OutBUS(upCOUNTER_2_BIN2BCD1_data_BUS_wire),
	.SC_upCOUNTER_CLOCK_50(BB_SYSTEM_CLOCK_50),
	.SC_upCOUNTER_RESET_InHigh(BB_SYSTEM_RESET_InHigh),
	.SC_upCOUNTER_upcount_InLow(STATEMACHINEFROGGER_load0_cwire)
);

endmodule
