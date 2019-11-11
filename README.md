# FROGGER
Quartus prime lite project

WINCOMPARATOR OPTIONS:
OPTION 1:
module CC_WINCOMPARATOR #(parameter WINCOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_WINCOMPARATOR_load_OutLow,
	CC_WINCOMPARATOR_data_OutBUS,
//////////// INPUTS //////////
	CC_WINCOMPARATOR_Frogger_InBUS,
	CC_WINCOMPARATOR_Register_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_WINCOMPARATOR_load_OutLow;
output	reg [WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_data_OutBUS;
input 	[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_Frogger_InBUS;
input		[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_Register_InBUS;

//=======================================================
//  REG/WIRE declarations
//=======================================================

//=======================================================
//  Structural coding
//=======================================================
always @(*)
begin
	if((CC_WINCOMPARATOR_Frogger_InBUS | CC_WINCOMPARATOR_Register_InBUS) != 8'b11011011)
		begin
		CC_WINCOMPARATOR_load_OutLow = 1'b0;
		CC_WINCOMPARATOR_data_OutBUS = CC_WINCOMPARATOR_Frogger_InBUS | CC_WINCOMPARATOR_Register_InBUS;		
		end		
	else
		CC_WINCOMPARATOR_load_OutLow = 1'b1;
end
endmodule

OPTION 2:

module CC_LEVELCOMPARATOR #(parameter LEVELCOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_LEVELCOMPARATOR_win_OutLow,
//	CC_LEVELCOMPARATOR_winLevel_OutLow,
//////////// INPUTS //////////
	CC_LEVELCOMPARATOR_data_InBUS
//	CC_LEVELCOMPARATOR_Register_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================

//=======================================================
//  PORT declarations
//=======================================================
output	reg CC_LEVELCOMPARATOR_win_OutLow;
output	reg CC_LEVELCOMPARATOR_winLevel_OutLow;
input 	[LEVELCOMPARATOR_DATAWIDTH-1:0] CC_LEVELCOMPARATOR_data_InBUS;
input 	[LEVELCOMPARATOR_DATAWIDTH-1:0] CC_LEVELCOMPARATOR_Register_InBUS;


//=======================================================
//  REG/WIRE declarations
//=======================================================

//=======================================================
//  Structural coding
//=======================================================
always @(*)
begin
	if(CC_LEVELCOMPARATOR_data_InBUS == 8'b11111111)
		begin
		CC_LEVELCOMPARATOR_winLevel_OutLow = 1'b0;
		CC_LEVELCOMPARATOR_win_OutLow = 1'b0;		
		end
	else if (CC_LEVELCOMPARATOR_data_InBUS != CC_LEVELCOMPARATOR_Register_InBUS)
    begin
			CC_LEVELCOMPARATOR_winLevel_OutLow = 1'b1;
			CC_LEVELCOMPARATOR_win_OutLow = 1'b0;		
		end
	else
		begin
		CC_LEVELCOMPARATOR_win_OutLow = 1'b1;
		CC_LEVELCOMPARATOR_winLevel_OutLow = 1'b1;
		end
end
endmodule

OPTION 3:
//=======================================================
//  MODULE Definition
//=======================================================
module CC_WINCOMPARATOR #(parameter WINCOMPARATOR_DATAWIDTH=8)(
//////////// OUTPUTS //////////
	CC_WINCOMPARATOR_win_OutLow,
	CC_WINCOMPARATOR_regOut,
//////////// INPUTS //////////
	CC_WINCOMPARATOR_data_InBUS
);
//=======================================================
//  PARAMETER declarations
//=======================================================
parameter WINCOMPARATOR_actual = 8'b11011011;
//=======================================================
//  PORT declarations
//=======================================================
output	[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_regOut;
output	reg CC_WINCOMPARATOR_win_OutLow;
input 	[WINCOMPARATOR_DATAWIDTH-1:0] CC_WINCOMPARATOR_data_InBUS;
//=======================================================
//  REG/WIRE declarations
//=======================================================
reg [DEADCOUNTER_DATAWIDTH-1:0] WINCOMPARATOR_Register;
reg WINCOMPARATOR_winstate;
//=======================================================
//  Structural coding
//=======================================================
always @(CC_WINCOMPARATOR_data_InBUS)
begin
	if(CC_WINCOMPARATOR_data_InBUS != WINCOMPARATOR_actual)
		begin
			WINCOMPARATOR_Register = CC_WINCOMPARATOR_data_InBUS;
			WINCOMPARATOR_actual = CC_WINCOMPARATOR_data_InBUS;
			WINCOMPARATOR_winstate = 1'b1;
			end
	if(CC_WINCOMPARATOR_data_InBUS == 8'b11111111)
		begin
			WINCOMPARATOR_Register = WINCOMPARATOR_actual;
			WINCOMPARATOR_actual = 8'b11011011;
			WINCOMPARATOR_winstate = 1'b0;		
			end
	else
		begin
			WINCOMPARATOR_Register = 8'b11011011;
			WINCOMPARATOR_winstate = 1'b1;
			end
end

//=======================================================
// OUTPUTs
//=======================================================
assign CC_WINCOMPARATOR_win_OutLow = WINCOMPARATOR_winstate;
assign CC_WINCOMPARATOR_regOut = WINCOMPARATOR_Register;

endmodule

&& 
