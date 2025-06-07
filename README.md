# PIPELINE-PROCESSOR-DESIGN

COMPANY: CODTECH IT SOLUTIONS

NAME: THAMMINENI BHAVANA

INTERN ID: CT04DM1127

DOMAIN: VLSI

DURATION: 4 WEEEKS

MENTOR: NEELA SANTOSH

module InstructionMemory(input [7:0] PC, output [15:0] instruction);
  reg [15:0] memory[0:255];
  initial begin
    memory[0] = 16'b0001001000010000; // LOAD R1, 0(R2)
    memory[1] = 16'b0010001100010100; // ADD R3, R1, R4
    memory[2] = 16'b0011001010110110; // SUB R5, R3, R6
  end
  assign instruction = memory[PC];
endmodule


module RegisterFile(
  input clk, input RegWrite,
  input [3:0] rs, rt, rd,
  input [15:0] writeData,
  output [15:0] readData1, readData2
);
  reg [15:0] regfile[0:15];
  assign readData1 = regfile[rs];
  assign readData2 = regfile[rt];
  always @(posedge clk)
    if (RegWrite) regfile[rd] <= writeData;
endmodule


module ALU(input [15:0] a, b, input [1:0] ALUOp, output reg [15:0] result);
  always @(*) begin
    case(ALUOp)
      2'b00: result = a + b;
      2'b01: result = a - b;
      default: result = 16'b0;
    endcase
  end
endmodule


module Memory(input clk, input MemRead, MemWrite,
              input [15:0] address, input [15:0] writeData,
              output reg [15:0] readData);
  reg [15:0] mem[0:255];
  always @(posedge clk) begin
    if (MemWrite) mem[address] <= writeData;
    if (MemRead) readData <= mem[address];
  end
endmodule
