module instruction_fetch_unit(
  input clk,
  input reset,
  input [31:0] imm_address,
  input [31:0] imm_address_jump,
  input beq,                          
  input bneq,                         
  input bge,                         
  input blt,                           
  input jump,                         
  output reg [31:0] pc,               
  output reg [31:0] pc_plus4 

  );
  
  // pc logic
always @(posedge clk) begin
    if (reset) begin
        pc <= 32'b0;
    end 
    else begin
        if (jump) begin
            pc <= pc + imm_address_jump;
        end
        else if (beq || bneq || bge || blt) begin
            pc <= pc + imm_address;
        end
        else begin
            pc <= pc + 4;
        end
    end
end
  
  // return address of pc
   always@(posedge clk)
            begin
            if(reset)
                begin
                    pc_plus4 <= 0;
                 end
                 
                else 
                    pc_plus4 <= pc + 4;

    end
    
endmodule   
