module control_unit(
                    input reset,                    //Reset signal 
                    input [6 :0] funct7,            //function 7 field 
                    input [2:0] funct3,             //function 3 field
                    input [6:0] opcode,             //opcode field         
                    output reg [5:0] alu_control,   //alu_control for controlling the alu module
                    output reg lb,                  //control signal for enabling load operation
                    output reg mem_to_reg,          //control signal for enabling data flow from memory to register
                    output reg bneq_control,        //control signal for enabling bneq operation
                    output reg beq_control,         //control signal for enabling beq operation
                    output reg bgeq_control,        //control signal for enabling bgeq operation
                    output reg blt_control,         //control signal for enabling blt operation
                    output reg jump,                //control signal for enabling jump operation
                    output reg sw,                  //control signal for enabling sw operation
                    output reg lui_control,          //control signal for enabling lui operation
                    output reg reg_write,
output reg mem_read,
output reg mem_write,
output reg alu_src
                   );
  // reset operation
  always@(reset)
    begin
      if(reset==1)
            alu_control   = 6'b0;
    mem_to_reg    = 0;
    reg_write     = 0;
    mem_read      = 0;
    mem_write     = 0;
    beq_control   = 0;
    bneq_control  = 0;
    bgeq_control  = 0;
    blt_control   = 0;
    jump          = 0;
    lui_control   = 0;
    alu_src = 0;
    end
  
  //Logic for analyzing the type of the instruction 
  always@(opcode or funct7 or funct3)
    begin
      // R - TYPE INSTRUCTIONS 
           if(opcode == 7'b0110011)
                    begin
                      reg_write = 1;
                      mem_to_reg = 0;
                    beq_control = 0;
                    bneq_control = 0;
                    bgeq_control = 0;
                    blt_control = 0;
                    jump = 0;
                    lui_control = 0;
                    alu_src = 0;

                case(funct3)
                3'b000: begin
                if (funct7 == 7'b0000000)
                    alu_control = 6'b000001; // ADD
                else if (funct7 == 7'b0100000)
                    alu_control = 6'b000010; // SUB
            end
            3'b001: alu_control = 6'b000011; // SLL
            3'b010: alu_control = 6'b000100; // SLT
            3'b011: alu_control = 6'b000101; // SLTU
            3'b100: alu_control = 6'b000110; // XOR
            3'b101: begin
                if (funct7 == 7'b0000000)
                    alu_control = 6'b000111; // SRL
                else
                    alu_control = 6'b001000; // SRA
            end
            3'b110: alu_control = 6'b001001; // OR
            3'b111: alu_control = 6'b001010; // AND
        endcase

                  end
//////////////////////////////////////////// END OF R- TYPE INSTRUCTION                    

/////////////////////////////////////////// I - TYPE INSTRUCTION  ///////////
                  else if(opcode == 7'b001_0011) // I TYPE
                  begin
                      reg_write = 1;
                      alu_src = 1;
                                        mem_to_reg = 0;
                  beq_control = 0;
                  bneq_control = 0;
                  jump = 0;
                  lb = 0;
                  sw = 0;
                       case(funct3)
                        3'b000: alu_control = 6'b000001; // ADDI
            3'b010: alu_control = 6'b000100; // SLTI
            3'b011: alu_control = 6'b000101; // SLTIU
            3'b100: alu_control = 6'b000110; // XORI
            3'b110: alu_control = 6'b001001; // ORI
            3'b111: alu_control = 6'b001010; // ANDI
            3'b001: alu_control = 6'b000011; // SLLI
            3'b101: begin
                if (funct7 == 7'b0000000)
                    alu_control = 6'b000111; // SRLI
                else
                    alu_control = 6'b001000; // SRAI
            end
        endcase
                            end
                        
                        else if(opcode == 7'b000_0011) // I type (load instructions)
                        begin
                        alu_src = 1;
    reg_write = 1;
    mem_to_reg = 1;
    mem_read   = 1;
                            case(funct3)
                                3'b000 :
                                        begin
                                            mem_to_reg = 1;           
                                            beq_control = 0;                  
                                            bneq_control = 0;                 
                                            jump = 0;  
                                            lb = 1;               
                                            alu_control = 6'b010011; // load byte
                                         end
                                3'b001 :
                                    begin
                                        alu_control = 6'b010100; // load_half
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;              
                                     end
                                     
                                3'b010 :begin
                                        
                                        alu_control = 6'b010101; //load_word
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;              
                                        end
                                        
                                3'b011 :begin

                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;              
                                        alu_control = 6'b010110;//load_byte unsigned
                                        end
                                                                                
                                3'b100 :begin
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;
                                        alu_control = 6'b010111; // load_half unsigned
                                        
                                        end


                                    endcase
                                    end
                                    
                          else if(opcode == 7'b0100_011)
                            begin
                            alu_src = 1;
                                reg_write = 0;
    mem_write = 1;
                            case(funct3)
                                3'b010 :begin
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;
                                        sw = 1;
                                        alu_control = 6'b011000; // store byte
                                        end
                                        
                                3'b110 :begin
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;
                                        alu_control = 6'b011001; // store half word
                                        end
                                        
                                3'b111 :begin
                                        mem_to_reg = 1;        
                                        beq_control = 0;               
                                        bneq_control = 0;              
                                        jump = 0;
                                        sw = 1;
                                        alu_control = 6'b011010; // store word
                                        end
                           endcase
                       end
                       
                       else if(opcode == 7'b110_0011)
                       begin
                       alu_src = 0;
                       reg_write = 0;
                            case(funct3)
                            //BRANCH EQUAL INSTRUCTION
                                3'b000 :
                                    begin
                                    alu_control = 6'b011011; 
                                    beq_control = 1;
                                    bneq_control = 0;
                                    blt_control = 0;
                                    bgeq_control = 0;
                                    
                                    end
                                    
                               //BRANCH UNEQUAL     
                                3'b001 :
                                    begin
                                    alu_control = 6'b011100; //branch unequal
                                    bneq_control = 1;
                                    beq_control = 0;
                                    blt_control = 0;
                                    bgeq_control = 0;
                                    
                                    end
                                    
                                 3'b010 :
                                    alu_control = 6'b011101; //branch less than
                                    
                                 3'b100 :
                                    
                                    begin
                                    alu_control = 6'b100000; //BRANCH LESS THAN INSTRUCTION
                                    blt_control = 1;
                                    beq_control = 0;
                                    bneq_control = 0;
                                    bgeq_control = 0;
                                    end
                                    
                                  3'b101 :
                                   begin
                                    alu_control = 6'b011111; // BRANCH IF GREATER THAN OR EQUAL TO 
                                    bgeq_control = 1;
                                    blt_control = 0;
                                    beq_control = 0;
                                    bneq_control = 0;
                                    end
                                 3'b110 :
                                    alu_control = 6'b100000; // branch greater than or equal to  unsigned 
                                    
                                endcase
                       end
                       
                       
                       
                       //LUI INSTRUCTION
                       
                       else if(opcode == 7'b011_0111)
                            begin
                               alu_control = 6'b100001;
                               lui_control = 1;
                                reg_write = 1; 
                               sw = 0;
                               lb = 0;
                               beq_control = 0;
                               blt_control = 0;
                               bneq_control = 0;
                               bgeq_control = 0;
                            end
                            
                       
                      //JUMP AND LINK OPERATION
                      
                       else if(opcode == 7'b110_1111)
                            begin
                                alu_control = 6'b100010;
                                jump = 1;
                                 reg_write = 1; 
                                lui_control = 0;
                                sw = 0;
                                lb = 0;
                                beq_control = 0;
                                blt_control = 0;
                                bneq_control = 0;
                                bgeq_control = 0;
                             end
                                                  
                           
               end 
                
        
endmodule
