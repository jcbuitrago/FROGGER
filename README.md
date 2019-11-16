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

//=======================================================
//  MODULE Definition
//=======================================================
module CC_BOTTOMSIDECOMPARATOR #(parameter BOTTOMSIDECOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_BOTTOMSIDECOMPARATOR_bottomside_OutLow,
//////////// INPUTS //////////
	CC_BOTTOMSIDECOMPARATOR_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_BOTTOMSIDECOMPARATOR_bottomside_OutLow;
input 	[BOTTOMSIDECOMPARATOR_DATAWIDTH-1:0] CC_BOTTOMSIDECOMPARATOR_data_InBUS;
//=======================================================
//  REG/WIRE declarations
//=======================================================
//=======================================================
//  Structural coding
//=======================================================
always @(CC_BOTTOMSIDECOMPARATOR_data_InBUS)
begin
	if( CC_BOTTOMSIDECOMPARATOR_data_InBUS == 8'b00000000)
		CC_BOTTOMSIDECOMPARATOR_bottomside_OutLow = 1'b1;
	else 
		CC_BOTTOMSIDECOMPARATOR_bottomside_OutLow = 1'b0;
end

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_RegHOUSESTYPE #(parameter RegHOUSESTYPE_DATAWIDTH=8, parameter DATA_FIXED_INITREGHOUSES=8'b11011011)(
	//////////// OUTPUTS //////////
	SC_RegHOUSESTYPE_data_OutBUS,
	//////////// INPUTS //////////
	SC_RegHOUSESTYPE_CLOCK_50,
	SC_RegHOUSESTYPE_RESET_InHigh,
	SC_RegHOUSESTYPE_clear_InLow, 
	SC_RegHOUSESTYPE_load_InLow, 
	SC_RegHOUSESTYPE_shiftselection_In,
	SC_RegHOUSESTYPE_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output		[RegHOUSESTYPE_DATAWIDTH-1:0]	SC_RegHOUSESTYPE_data_OutBUS;
input		SC_RegHOUSESTYPE_CLOCK_50;
input		SC_RegHOUSESTYPE_RESET_InHigh;
input		SC_RegHOUSESTYPE_clear_InLow;
input		SC_RegHOUSESTYPE_load_InLow;	
input		[1:0] SC_RegHOUSESTYPE_shiftselection_In;
input		[RegHOUSESTYPE_DATAWIDTH-1:0]	SC_RegHOUSESTYPE_data_InBUS;

//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [RegHOUSESTYPE_DATAWIDTH-1:0] RegHOUSESTYPE_Register;
reg [RegHOUSESTYPE_DATAWIDTH-1:0] RegHOUSESTYPE_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (SC_RegHOUSESTYPE_clear_InLow == 1'b0)
		RegHOUSESTYPE_Signal = DATA_FIXED_INITREGHOUSES;
	else if (SC_RegHOUSESTYPE_load_InLow == 1'b0)
		RegHOUSESTYPE_Signal = SC_RegHOUSESTYPE_data_InBUS;
	else if (SC_RegHOUSESTYPE_shiftselection_In == 2'b01)
		RegHOUSESTYPE_Signal = {RegHOUSESTYPE_Register[RegHOUSESTYPE_DATAWIDTH-2:0],RegHOUSESTYPE_Register[RegHOUSESTYPE_DATAWIDTH-1]};
	else if (SC_RegHOUSESTYPE_shiftselection_In== 2'b10)
		RegHOUSESTYPE_Signal = {RegHOUSESTYPE_Register[0],RegHOUSESTYPE_Register[RegHOUSESTYPE_DATAWIDTH-1:1]};
	else
		RegHOUSESTYPE_Signal = RegHOUSESTYPE_Register;
	end	
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_RegHOUSESTYPE_CLOCK_50, posedge SC_RegHOUSESTYPE_RESET_InHigh)
begin
	if (SC_RegHOUSESTYPE_RESET_InHigh == 1'b1)
		RegHOUSESTYPE_Register <= 0;
	else
		RegHOUSESTYPE_Register <= RegHOUSESTYPE_Signal;
end
//=======================================================
//  Outputs
//=======================================================
//OUTPUT LOGIC: COMBINATIONAL
assign SC_RegHOUSESTYPE_data_OutBUS = RegHOUSESTYPE_Register;

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_upSPEEDCOUNTER #(parameter upSPEEDCOUNTER_DATAWIDTH=8)(
	//////////// OUTPUTS //////////
	SC_upSPEEDCOUNTER_data_OutBUS,
	//////////// INPUTS //////////
	SC_upSPEEDCOUNTER_CLOCK_50,
	SC_upSPEEDCOUNTER_RESET_InHigh,
	SC_upSPEEDCOUNTER_upcount_InLow
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output		[upSPEEDCOUNTER_DATAWIDTH-1:0]	SC_upSPEEDCOUNTER_data_OutBUS;
input		SC_upSPEEDCOUNTER_CLOCK_50;
input		SC_upSPEEDCOUNTER_RESET_InHigh;
input		SC_upSPEEDCOUNTER_upcount_InLow;

//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [upSPEEDCOUNTER_DATAWIDTH-1:0] upSPEEDCOUNTER_Register;
reg [upSPEEDCOUNTER_DATAWIDTH-1:0] upSPEEDCOUNTER_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (SC_upSPEEDCOUNTER_upcount_InLow == 1'b0)
		upSPEEDCOUNTER_Signal = upSPEEDCOUNTER_Register + 1'b1;
	else
		upSPEEDCOUNTER_Signal = upSPEEDCOUNTER_Register;
	end	
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_upSPEEDCOUNTER_CLOCK_50, posedge SC_upSPEEDCOUNTER_RESET_InHigh)
begin
	if (SC_upSPEEDCOUNTER_RESET_InHigh  == 1'b1)
		upSPEEDCOUNTER_Register <= 0;
	else
		upSPEEDCOUNTER_Register <= upSPEEDCOUNTER_Signal;
end
//=======================================================
//  Outputs
//=======================================================
//OUTPUT LOGIC: COMBINATIONAL
assign SC_upSPEEDCOUNTER_data_OutBUS = upSPEEDCOUNTER_Register;

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_STATEMACHINECARS (
	//////////// OUTPUTS //////////
	SC_STATEMACHINECARS_clear_OutLow,
	SC_STATEMACHINECARS_load_OutLow,
	SC_STATEMACHINECARS_shiftselection_R_Out,
	SC_STATEMACHINECARS_shiftselection_L_Out,
	SC_STATEMACHINECARS_upcount_out,
	//////////// INPUTS //////////
	SC_STATEMACHINECARS_CLOCK_50,
	SC_STATEMACHINECARS_RESET_InHigh,
	SC_STATEMACHINECARS_startButton_InLow,
	SC_STATEMACHINECARS_T0_InLow,
	SC_STATEMACHINECARS_win_InLow
);	
//=======================================================
//  PARAMETER declarations
//=======================================================
// states declaration
localparam STATE_RESET_0									= 0;
localparam STATE_START_0									= 1;
localparam STATE_CHECK_0									= 2;
localparam STATE_INIT_0										= 3;
localparam STATE_SHIFT_0									= 4;
localparam STATE_COUNT_0									= 5;
localparam STATE_CHECK_1									= 6;
localparam STATE_LOAD_0										= 7;
//=======================================================
//  PORT declarations
//=======================================================
output reg		SC_STATEMACHINECARS_clear_OutLow;
output reg		SC_STATEMACHINECARS_load_OutLow;
output reg		[1:0] SC_STATEMACHINECARS_shiftselection_R_Out;
output reg		[1:0] SC_STATEMACHINECARS_shiftselection_L_Out;
output reg 		SC_STATEMACHINECARS_upcount_out;
input			SC_STATEMACHINECARS_CLOCK_50;
input 		SC_STATEMACHINECARS_RESET_InHigh;
input			SC_STATEMACHINECARS_startButton_InLow;
input			SC_STATEMACHINECARS_T0_InLow;
input			SC_STATEMACHINECARS_win_InLow;
//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [3:0] STATE_Register;
reg [3:0] STATE_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
// NEXT STATE LOGIC : COMBINATIONAL
always @(*)
begin
	case (STATE_Register)
		STATE_RESET_0: STATE_Signal = STATE_START_0;
		STATE_START_0: STATE_Signal = STATE_CHECK_0;
		STATE_CHECK_0: if (SC_STATEMACHINECARS_startButton_InLow == 1'b0 ) STATE_Signal = STATE_INIT_0;
						else if (SC_STATEMACHINECARS_T0_InLow == 1'b0) STATE_Signal = STATE_SHIFT_0;
						else if (SC_STATEMACHINECARS_win_InLow == 1'b0) STATE_Signal = STATE_LOAD_0;
						else STATE_Signal = STATE_COUNT_0;
		STATE_LOAD_0:  STATE_Signal = STATE_CHECK_0;
		STATE_INIT_0:	STATE_Signal = STATE_CHECK_1;
		STATE_SHIFT_0: 	STATE_Signal = STATE_COUNT_0;
		STATE_COUNT_0: 	STATE_Signal = STATE_CHECK_0;
		
		STATE_CHECK_1: if (SC_STATEMACHINECARS_startButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else STATE_Signal = STATE_CHECK_0;

		default : 		STATE_Signal = STATE_CHECK_0;
	endcase
end
// STATE REGISTER : SEQUENTIAL
always @ ( posedge SC_STATEMACHINECARS_CLOCK_50 , posedge SC_STATEMACHINECARS_RESET_InHigh)
begin
	if (SC_STATEMACHINECARS_RESET_InHigh == 1'b1)
		STATE_Register <= STATE_RESET_0;
	else
		STATE_Register <= STATE_Signal;
end
//=======================================================
//  Outputs
//=======================================================
// OUTPUT LOGIC : COMBINATIONAL
always @ (*)
begin
	case (STATE_Register)
//=========================================================
// STATE_RESET
//=========================================================
	STATE_RESET_0 :	
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11; 
			SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_START
//=========================================================
	STATE_START_0 :	
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11;
			SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_CHECK
//=========================================================
	STATE_CHECK_0 :
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11;
			SC_STATEMACHINECARS_shiftselection_L_Out  = 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_CHECK
//=========================================================
	STATE_CHECK_1 :
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11; 
			SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_INIT
//=========================================================
	STATE_INIT_0 :	
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b0;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11;
			SC_STATEMACHINECARS_shiftselection_L_Out  = 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_SHIFT
//=========================================================
	STATE_SHIFT_0 :
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b10;
		   SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b01;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// STATE_COUNT_0
//=========================================================
	STATE_COUNT_0 :	
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11;
			SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b0;
		end
		
	STATE_LOAD_0 :
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b0;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11; 
			SC_STATEMACHINECARS_shiftselection_L_Out  = 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
//=========================================================
// DEFAULT
//=========================================================
	default :
		begin
			SC_STATEMACHINECARS_clear_OutLow = 1'b1;
			SC_STATEMACHINECARS_load_OutLow = 1'b1;
			SC_STATEMACHINECARS_shiftselection_R_Out  = 2'b11;
			SC_STATEMACHINECARS_shiftselection_L_Out	= 2'b11;
			SC_STATEMACHINECARS_upcount_out = 1'b1;
		end
	endcase
end
endmodule


//=======================================================
//  MODULE Definition
//=======================================================
module SC_RegCARSTYPE #(parameter RegCARSTYPE_DATAWIDTH=8, parameter DATA_FIXED_INITREGCARS=8'b00000000)(
	//////////// OUTPUTS //////////
	SC_RegCARSTYPE_data_OutBUS,
	//////////// INPUTS //////////
	SC_RegCARSTYPE_CLOCK_50,
	SC_RegCARSTYPE_RESET_InHigh,
	SC_RegCARSTYPE_clear_InLow, 
	SC_RegCARSTYPE_load_InLow, 
	SC_RegCARSTYPE_shiftselection_In,
	SC_RegCARSTYPE_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output		[RegCARSTYPE_DATAWIDTH-1:0]	SC_RegCARSTYPE_data_OutBUS;
input		SC_RegCARSTYPE_CLOCK_50;
input		SC_RegCARSTYPE_RESET_InHigh;
input		SC_RegCARSTYPE_clear_InLow;
input		SC_RegCARSTYPE_load_InLow;	
input		[1:0] SC_RegCARSTYPE_shiftselection_In;
input		[RegCARSTYPE_DATAWIDTH-1:0]	SC_RegCARSTYPE_data_InBUS;

//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [RegCARSTYPE_DATAWIDTH-1:0] RegCARSTYPE_Register;
reg [RegCARSTYPE_DATAWIDTH-1:0] RegCARSTYPE_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (SC_RegCARSTYPE_clear_InLow == 1'b0)
		RegCARSTYPE_Signal = DATA_FIXED_INITREGCARS;
	else if (SC_RegCARSTYPE_load_InLow == 1'b0)
		RegCARSTYPE_Signal = SC_RegCARSTYPE_data_InBUS;
	else if (SC_RegCARSTYPE_shiftselection_In == 2'b01)
		RegCARSTYPE_Signal = {RegCARSTYPE_Register[RegCARSTYPE_DATAWIDTH-2:0],RegCARSTYPE_Register[RegCARSTYPE_DATAWIDTH-1]};
	else if (SC_RegCARSTYPE_shiftselection_In== 2'b10)
		RegCARSTYPE_Signal = {RegCARSTYPE_Register[0],RegCARSTYPE_Register[RegCARSTYPE_DATAWIDTH-1:1]};
	else
		RegCARSTYPE_Signal = RegCARSTYPE_Register;
	end	
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_RegCARSTYPE_CLOCK_50, posedge SC_RegCARSTYPE_RESET_InHigh)
begin
	if (SC_RegCARSTYPE_RESET_InHigh == 1'b1)
		RegCARSTYPE_Register <= 0;
	else
		RegCARSTYPE_Register <= RegCARSTYPE_Signal;
end
//=======================================================
//  Outputs
//=======================================================
//OUTPUT LOGIC: COMBINATIONAL
assign SC_RegCARSTYPE_data_OutBUS = RegCARSTYPE_Register;

endmodule


//=======================================================
//  MODULE Definition
//=======================================================
module CC_SPEEDCOMPARATOR #(parameter SPEEDCOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_SPEEDCOMPARATOR_T0_OutLow,
//////////// INPUTS //////////
	CC_SPEEDCOMPARATOR_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_SPEEDCOMPARATOR_T0_OutLow;
input 	[SPEEDCOMPARATOR_DATAWIDTH-1:0] CC_SPEEDCOMPARATOR_data_InBUS;
//=======================================================
//  REG/WIRE declarations
//=======================================================
//=======================================================
//  Structural coding
//=======================================================
always @(CC_SPEEDCOMPARATOR_data_InBUS)
begin
	if( CC_SPEEDCOMPARATOR_data_InBUS == 22'b1111111111111111111111)
		CC_SPEEDCOMPARATOR_T0_OutLow = 1'b0;
	else 
		CC_SPEEDCOMPARATOR_T0_OutLow = 1'b1;
end

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_RegFROGGERTYPE #(parameter RegFROGGERTYPE_DATAWIDTH=8, parameter DATA_FIXED_INITREGFROGGER=8'b00000000)(
	//////////// OUTPUTS //////////
	SC_RegFROGGERTYPE_data_OutBUS,
	
	//////////// INPUTS //////////
	SC_RegFROGGERTYPE_CLOCK_50,
	SC_RegFROGGERTYPE_RESET_InHigh,
	SC_RegFROGGERTYPE_clear_InLow, 
	SC_RegFROGGERTYPE_load0_InLow, 
	SC_RegFROGGERTYPE_load1_InLow, 
	SC_RegFROGGERTYPE_shiftselection_In,
	SC_RegFROGGERTYPE_data0_InBUS,
	SC_RegFROGGERTYPE_data1_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output		[RegFROGGERTYPE_DATAWIDTH-1:0]	SC_RegFROGGERTYPE_data_OutBUS;
input		SC_RegFROGGERTYPE_CLOCK_50;
input		SC_RegFROGGERTYPE_RESET_InHigh;
input		SC_RegFROGGERTYPE_clear_InLow;
input		SC_RegFROGGERTYPE_load0_InLow;	
input		SC_RegFROGGERTYPE_load1_InLow;	
input		[1:0] SC_RegFROGGERTYPE_shiftselection_In;
input		[RegFROGGERTYPE_DATAWIDTH-1:0]	SC_RegFROGGERTYPE_data0_InBUS;
input		[RegFROGGERTYPE_DATAWIDTH-1:0]	SC_RegFROGGERTYPE_data1_InBUS;

//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [RegFROGGERTYPE_DATAWIDTH-1:0] RegFROGGERTYPE_Register;
reg [RegFROGGERTYPE_DATAWIDTH-1:0] RegFROGGERTYPE_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (SC_RegFROGGERTYPE_clear_InLow == 1'b0)
		RegFROGGERTYPE_Signal = DATA_FIXED_INITREGFROGGER;
	else if (SC_RegFROGGERTYPE_load0_InLow == 1'b0)
		RegFROGGERTYPE_Signal = SC_RegFROGGERTYPE_data0_InBUS;
	else if (SC_RegFROGGERTYPE_load1_InLow == 1'b0)
		RegFROGGERTYPE_Signal = SC_RegFROGGERTYPE_data1_InBUS;
	else if (SC_RegFROGGERTYPE_shiftselection_In == 2'b01)
		RegFROGGERTYPE_Signal = {RegFROGGERTYPE_Register[RegFROGGERTYPE_DATAWIDTH-2:0],RegFROGGERTYPE_Register[RegFROGGERTYPE_DATAWIDTH-1]};
	else if (SC_RegFROGGERTYPE_shiftselection_In == 2'b10)
		RegFROGGERTYPE_Signal = {RegFROGGERTYPE_Register[0],RegFROGGERTYPE_Register[RegFROGGERTYPE_DATAWIDTH-1:1]};
	else
		RegFROGGERTYPE_Signal = RegFROGGERTYPE_Register;
	end	
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_RegFROGGERTYPE_CLOCK_50, posedge SC_RegFROGGERTYPE_RESET_InHigh)
begin
	if (SC_RegFROGGERTYPE_RESET_InHigh == 1'b1)
		RegFROGGERTYPE_Register <= 0;
	else
		RegFROGGERTYPE_Register <= RegFROGGERTYPE_Signal;
end
//=======================================================
//  Outputs
//=======================================================
//OUTPUT LOGIC: COMBINATIONAL
assign SC_RegFROGGERTYPE_data_OutBUS = RegFROGGERTYPE_Register;

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_STATEMACHINEFROGGER (
	//////////// OUTPUTS //////////
	SC_STATEMACHINEFROGGER_clear_OutLow,
	SC_STATEMACHINEFROGGER_load0_OutLow,
	SC_STATEMACHINEFROGGER_load1_OutLow,
	SC_STATEMACHINEFROGGER_shiftselection_Out,
	//////////// INPUTS //////////
	SC_STATEMACHINEFROGGER_CLOCK_50,
	SC_STATEMACHINEFROGGER_RESET_InHigh,
	SC_STATEMACHINEFROGGER_startButton_InLow,
	SC_STATEMACHINEFROGGER_upButton_InLow,
	SC_STATEMACHINEFROGGER_downButton_InLow,
	SC_STATEMACHINEFROGGER_leftButton_InLow,
	SC_STATEMACHINEFROGGER_rightButton_InLow,
	SC_STATEMACHINEFROGGER_bottomsidecomparator_InLow,
	SC_STATEMACHINEFROGGER_win_InLow,
	SC_STATEMACHINEFROGGER_dead_InLow
);	
//=======================================================
//  PARAMETER declarations
//=======================================================
// states declaration
localparam STATE_RESET_0									= 0;
localparam STATE_START_0									= 1;
localparam STATE_CHECK_0									= 2;
localparam STATE_INIT_0										= 3;
localparam STATE_UP_0										= 4;
localparam STATE_DOWN_0										= 5; 
localparam STATE_LEFT_0										= 6; 
localparam STATE_RIGHT_0									= 7;
localparam STATE_CHECK_1									= 8;
//=======================================================
//  PORT declarations
//=======================================================
output reg		SC_STATEMACHINEFROGGER_clear_OutLow;
output reg		SC_STATEMACHINEFROGGER_load0_OutLow;
output reg		SC_STATEMACHINEFROGGER_load1_OutLow;
output reg		[1:0] SC_STATEMACHINEFROGGER_shiftselection_Out;
input			SC_STATEMACHINEFROGGER_CLOCK_50;
input 			SC_STATEMACHINEFROGGER_RESET_InHigh;
input			SC_STATEMACHINEFROGGER_startButton_InLow;
input			SC_STATEMACHINEFROGGER_upButton_InLow;
input			SC_STATEMACHINEFROGGER_downButton_InLow;
input			SC_STATEMACHINEFROGGER_leftButton_InLow;
input			SC_STATEMACHINEFROGGER_rightButton_InLow;
input			SC_STATEMACHINEFROGGER_bottomsidecomparator_InLow;
input			SC_STATEMACHINEFROGGER_win_InLow;
input			SC_STATEMACHINEFROGGER_dead_InLow;
//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [3:0] STATE_Register;
reg [3:0] STATE_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
// NEXT STATE LOGIC : COMBINATIONAL
always @(*)
begin
	case (STATE_Register)
		STATE_RESET_0: STATE_Signal = STATE_START_0;
		STATE_START_0: STATE_Signal = STATE_CHECK_0;
		STATE_CHECK_0: if (SC_STATEMACHINEFROGGER_startButton_InLow == 1'b0 || SC_STATEMACHINEFROGGER_win_InLow == 1'b0 || SC_STATEMACHINEFROGGER_dead_InLow == 1'b0) STATE_Signal = STATE_INIT_0;
						else if (SC_STATEMACHINEFROGGER_upButton_InLow == 1'b0) STATE_Signal = STATE_UP_0;
						else if (SC_STATEMACHINEFROGGER_downButton_InLow == 1'b0 & (SC_STATEMACHINEFROGGER_bottomsidecomparator_InLow == 1'b1)) STATE_Signal = STATE_DOWN_0;
						else if (SC_STATEMACHINEFROGGER_leftButton_InLow == 1'b0) STATE_Signal = STATE_LEFT_0;
						else if (SC_STATEMACHINEFROGGER_rightButton_InLow == 1'b0) STATE_Signal = STATE_RIGHT_0;
						else STATE_Signal = STATE_CHECK_0;
		STATE_INIT_0: 	STATE_Signal = STATE_CHECK_1;
		STATE_UP_0: 	STATE_Signal = STATE_CHECK_1;
		STATE_DOWN_0: 	STATE_Signal = STATE_CHECK_1;
		STATE_LEFT_0:  	STATE_Signal = STATE_CHECK_1;
		STATE_RIGHT_0:  STATE_Signal = STATE_CHECK_1;

		STATE_CHECK_1: if (SC_STATEMACHINEFROGGER_startButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else if (SC_STATEMACHINEFROGGER_upButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else if (SC_STATEMACHINEFROGGER_downButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else if (SC_STATEMACHINEFROGGER_leftButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else if (SC_STATEMACHINEFROGGER_rightButton_InLow == 1'b0) STATE_Signal = STATE_CHECK_1;
						else STATE_Signal = STATE_CHECK_0;

		default : 		STATE_Signal = STATE_CHECK_0;
	endcase
end
// STATE REGISTER : SEQUENTIAL
always @ ( posedge SC_STATEMACHINEFROGGER_CLOCK_50 , posedge SC_STATEMACHINEFROGGER_RESET_InHigh)
begin
	if (SC_STATEMACHINEFROGGER_RESET_InHigh == 1'b1)
		STATE_Register <= STATE_RESET_0;
	else
		STATE_Register <= STATE_Signal;
end
//=======================================================
//  Outputs
//=======================================================
// OUTPUT LOGIC : COMBINATIONAL
always @ (*)
begin
	case (STATE_Register)
//=========================================================
// STATE_RESET
//=========================================================
	STATE_RESET_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_START
//=========================================================
	STATE_START_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_CHECK
//=========================================================
	STATE_CHECK_0 :
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_CHECK
//=========================================================
	STATE_CHECK_1 :
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_INIT_0
//=========================================================
	STATE_INIT_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b0;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_UP_0
//=========================================================
	STATE_UP_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b0;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_DOWN_0
//=========================================================
	STATE_DOWN_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b0;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
//=========================================================
// STATE_LEFT_0
//=========================================================
	STATE_LEFT_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b01; 
		end
//=========================================================
// STATE_RIGHT_0
//=========================================================
	STATE_RIGHT_0 :	
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b10; 
		end

//=========================================================
// DEFAULT
//=========================================================
	default :
		begin
			SC_STATEMACHINEFROGGER_clear_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load0_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_load1_OutLow = 1'b1;
			SC_STATEMACHINEFROGGER_shiftselection_Out  = 2'b11; 
		end
	endcase
end
endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module CC_WINCOMPARATOR #(parameter WINCOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_WINCOMPARATOR_win_OutLow,
	CC_WINCOMPARATOR_winLevel_OutLow,
	CC_WINCOMPARATOR_data_OutBUS,
//////////// INPUTS //////////
	CC_WINCOMPARATOR_data_InBUS,
	SC_WINCOMPARATOR_CLOCK_50,
	SC_WINCOMPARATOR_RESET_InHigh,
	CC_WINCOMPARATOR_Regster_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_WINCOMPARATOR_win_OutLow;
output	reg CC_WINCOMPARATOR_winLevel_OutLow;
output	[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_data_OutBUS;
input 	[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_data_InBUS;
input		[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_Regster_InBUS;
input		SC_WINCOMPARATOR_RESET_InHigh;
input		SC_WINCOMPARATOR_CLOCK_50;
//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [WINCOMPARATOR_DATAWIDTH-1:0]WINCOMPARATOR_Signal;
reg [WINCOMPARATOR_DATAWIDTH-1:0]WINCOMPARATOR_Register;
//=======================================================
//  Structural coding
//=======================================================
always @(CC_WINCOMPARATOR_data_InBUS, CC_WINCOMPARATOR_Regster_InBUS)
begin
	if(CC_WINCOMPARATOR_data_InBUS == 8'b11111111)
		begin
		WINCOMPARATOR_Signal = 8'b11011011;
		CC_WINCOMPARATOR_winLevel_OutLow = 1'b0;
		CC_WINCOMPARATOR_win_OutLow = 1'b0;
		end
	else if( CC_WINCOMPARATOR_data_InBUS != CC_WINCOMPARATOR_Regster_InBUS )
		begin
		WINCOMPARATOR_Signal = CC_WINCOMPARATOR_data_InBUS;
		CC_WINCOMPARATOR_win_OutLow = 1'b0;
		CC_WINCOMPARATOR_winLevel_OutLow = 1'b1;
		end
	else 
		begin
		WINCOMPARATOR_Signal = 8'b11011011;
		CC_WINCOMPARATOR_winLevel_OutLow = 1'b1;
		CC_WINCOMPARATOR_win_OutLow = 1'b1;
		end
end
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_WINCOMPARATOR_CLOCK_50, posedge SC_WINCOMPARATOR_RESET_InHigh)
begin
	if (SC_WINCOMPARATOR_RESET_InHigh  == 1'b1)
		WINCOMPARATOR_Register <= 0;
	else
		WINCOMPARATOR_Register <= WINCOMPARATOR_Signal;
end
assign CC_WINCOMPARATOR_data_OutBUS = WINCOMPARATOR_Register;
endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module CC_LEVELSTYPE #(parameter LEVELSTYPE_DATAWIDTH=8)(
	//////////// OUTPUTS //////////
	CC_LEVELSTYPE_regcars1_OutBUS,
	CC_LEVELSTYPE_regcars2_OutBUS,
	CC_LEVELSTYPE_regcars3_OutBUS,
	CC_LEVELSTYPE_regcars4_OutBUS,
	CC_LEVELSTYPE_regcars5_OutBUS,
	CC_LEVELSTYPE_regcars6_OutBUS,
	CC_LEVELSTYPE_regHouse_OutBUS,
	//////////// INPUTS //////////
	CC_LEVELSTYPE_levelcount_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg	[LEVELSTYPE_DATAWIDTH-1:0] CC_LEVELSTYPE_regcars1_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regcars2_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regcars3_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regcars4_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regcars5_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regcars6_OutBUS;
output	reg	[LEVELSTYPE_DATAWIDTH-1:0]	CC_LEVELSTYPE_regHouse_OutBUS;
input		[1:0] CC_LEVELSTYPE_levelcount_InBUS;
//=======================================================
//  REG/WIRE declarations
//=======================================================
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (CC_LEVELSTYPE_levelcount_InBUS == 2'b00)
		begin
		CC_LEVELSTYPE_regcars1_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars2_OutBUS = 8'b01100000;
		CC_LEVELSTYPE_regcars3_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars4_OutBUS = 8'b00011000;
		CC_LEVELSTYPE_regcars5_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars6_OutBUS = 8'b00000011;
		CC_LEVELSTYPE_regHouse_OutBUS = 8'b11011011;
		end
	else if (CC_LEVELSTYPE_levelcount_InBUS == 2'b01)
		begin
		CC_LEVELSTYPE_regcars1_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars2_OutBUS = 8'b11100000;
		CC_LEVELSTYPE_regcars3_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars4_OutBUS = 8'b00000111;
		CC_LEVELSTYPE_regcars5_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars6_OutBUS = 8'b00111000;
		CC_LEVELSTYPE_regHouse_OutBUS = 8'b11100111;		
		end
	else if (CC_LEVELSTYPE_levelcount_InBUS == 2'b10)
		begin
		CC_LEVELSTYPE_regcars1_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars2_OutBUS = 8'b11000000;
		CC_LEVELSTYPE_regcars3_OutBUS = 8'b00001110;
		CC_LEVELSTYPE_regcars4_OutBUS = 8'b01110000;
		CC_LEVELSTYPE_regcars5_OutBUS = 8'b00001100;
		CC_LEVELSTYPE_regcars6_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regHouse_OutBUS = 8'b10111101;		
		end
	else if (CC_LEVELSTYPE_levelcount_InBUS == 2'b11)
		begin
		CC_LEVELSTYPE_regcars1_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars2_OutBUS = 8'b11100000;
		CC_LEVELSTYPE_regcars3_OutBUS = 8'b00001100;
		CC_LEVELSTYPE_regcars4_OutBUS = 8'b01100000;
		CC_LEVELSTYPE_regcars5_OutBUS = 8'b00000011;
		CC_LEVELSTYPE_regcars6_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regHouse_OutBUS = 8'b01111110;		
		end
	else
		begin
		CC_LEVELSTYPE_regcars1_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars2_OutBUS = 8'b01100000;
		CC_LEVELSTYPE_regcars3_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars4_OutBUS = 8'b00011000;
		CC_LEVELSTYPE_regcars5_OutBUS = 8'b00000000;
		CC_LEVELSTYPE_regcars6_OutBUS = 8'b00000011;
		CC_LEVELSTYPE_regHouse_OutBUS = 8'b11011011;		
		end
	end	
endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module SC_LEVELCOUNTER #(parameter LEVELCOUNTER_DATAWIDTH=2)(
	//////////// OUTPUTS //////////
	SC_LEVELCOUNTER_data_OutBUS,
	//////////// INPUTS //////////
	SC_LEVELCOUNTER_CLOCK_50,
	SC_LEVELCOUNTER_RESET_InHigh,
	SC_LEVELCOUNTER_upcount_InLow
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output		[1:0]SC_LEVELCOUNTER_data_OutBUS;
input		SC_LEVELCOUNTER_CLOCK_50;
input		SC_LEVELCOUNTER_RESET_InHigh;
input		SC_LEVELCOUNTER_upcount_InLow;

//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [1:0] LEVELCOUNTER_Register;
reg [1:0] LEVELCOUNTER_Signal;
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (SC_LEVELCOUNTER_upcount_InLow == 1'b0)
		LEVELCOUNTER_Signal = LEVELCOUNTER_Register + 1'b1;
	else
		LEVELCOUNTER_Signal = LEVELCOUNTER_Register;
	end	
//STATE REGISTER: SEQUENTIAL
always @(posedge SC_LEVELCOUNTER_CLOCK_50, posedge SC_LEVELCOUNTER_RESET_InHigh)
begin
	if (SC_LEVELCOUNTER_RESET_InHigh  == 1'b1)
		LEVELCOUNTER_Register <= 0;
	else
		LEVELCOUNTER_Register <= LEVELCOUNTER_Signal;
end
//=======================================================
//  Outputs
//=======================================================
//OUTPUT LOGIC: COMBINATIONAL
assign SC_LEVELCOUNTER_data_OutBUS = LEVELCOUNTER_Register;

endmodule

//=======================================================
//  MODULE Definition
//=======================================================
module CC_DEADCOUNTER #(parameter DEADCOUNTER_DATAWIDTH=8)(
	//////////// OUTPUTS //////////
	CC_DEADCOUNTER_deadstate_OutLow,
	//////////// INPUTS //////////
	CC_DEADCOUNTER_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_DEADCOUNTER_deadstate_OutLow;
input		[DEADCOUNTER_DATAWIDTH-1:0]	CC_DEADCOUNTER_data_InBUS;

//=======================================================
//  REG/WIRE declarations
//=======================================================
//=======================================================
//  Structural coding
//=======================================================
//INPUT LOGIC: COMBINATIONAL
always @(*)
begin
	if (CC_DEADCOUNTER_data_InBUS != 8'b00000000)
		CC_DEADCOUNTER_deadstate_OutLow = 1'b0;
	else
		CC_DEADCOUNTER_deadstate_OutLow = 1'b1;
end	
endmodule
