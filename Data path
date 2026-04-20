module data_path(
    input clk,
    input rst,

    input [4:0] read_reg_num1,
    input [4:0] read_reg_num2,
    input [4:0] write_reg_num1,
    input [5:0] alu_control,
    input jump, beq_control, bne_control,
    input [31:0] imm_val,
    input [3:0] shamt,
    input lb,
    input sw,
    input bgeq_control,
    input blt_control,
    input lui_control,
    input [31:0] imm_val_lui,
    input mem_to_reg,
input reg_write,
input [31:0] pc_plus4_in,
input alu_src,
    output [4:0] read_data_addr_dm,
    output beq, bneq, bge, blt
);

  

    // ================= ID stage =================
    wire [31:0] read_data1, read_data2;
    wire [4:0]  read_data_addr_dm_2;
    wire [31:0] data_out_2_dm;

    register_file rfu (
        .clk(clk),
        .rst(rst),
        .read_reg_num1(read_reg_num1),
        .read_reg_num2(read_reg_num2),
        .write_reg_num1(MEM_WB_write_reg_num1),
        .write_data_dm(write_back_data),
        .reg_write(MEM_WB_reg_write),
        .lb(MEM_WB_lb),
        .lui_control(lui_control),
        .lui_imm_val(imm_val_lui),
        .jump(jump),
        .read_data1(read_data1),
        .read_data2(read_data2),
        .read_data_addr_dm(read_data_addr_dm_2),
        .data_out_2_dm(data_out_2_dm),
        .sw(MEM_WB_sw)
    );

    // ================= ID/EX =================
    reg [31:0] ID_EX_read_data1, ID_EX_read_data2, ID_EX_imm_val;
    reg [3:0]  ID_EX_shamt;
    reg [4:0]  ID_EX_write_reg_num1;
    reg [5:0]  ID_EX_alu_control;
    reg        ID_EX_beq_control, ID_EX_bne_control;
    reg        ID_EX_bgeq_control, ID_EX_blt_control;
    reg        ID_EX_lb, ID_EX_sw, ID_EX_mem_to_reg;
    reg ID_EX_reg_write;
    reg [31:0] ID_EX_pc_plus4;
    reg ID_EX_alu_src;

    always @(posedge clk) begin
        if (rst) begin
            ID_EX_read_data1 <= 0;
            ID_EX_read_data2 <= 0;
            ID_EX_imm_val <= 0;
            ID_EX_shamt <= 0;
            ID_EX_write_reg_num1 <= 0;
            ID_EX_alu_control <= 0;
            ID_EX_beq_control <= 0;
            ID_EX_bne_control <= 0;
            ID_EX_bgeq_control <= 0;
            ID_EX_blt_control <= 0;
            ID_EX_lb <= 0;
            ID_EX_sw <= 0;
            ID_EX_mem_to_reg <= 0;
            ID_EX_reg_write <= 0;
            ID_EX_pc_plus4 <= 0;
            ID_EX_alu_src <= 0;
        end else begin
            ID_EX_read_data1 <= read_data1;
            ID_EX_read_data2 <= read_data2;
            ID_EX_imm_val <= imm_val;
            ID_EX_shamt <= shamt;
            ID_EX_write_reg_num1 <= write_reg_num1;
            ID_EX_alu_control <= alu_control;
            ID_EX_beq_control <= beq_control;
            ID_EX_bne_control <= bne_control;
            ID_EX_bgeq_control <= bgeq_control;
            ID_EX_blt_control <= blt_control;
            ID_EX_lb <= lb;
            ID_EX_sw <= sw;
            ID_EX_mem_to_reg <= mem_to_reg;
            ID_EX_reg_write <= reg_write;
            ID_EX_pc_plus4 <= pc_plus4_in;
            ID_EX_alu_src <= alu_src;
        end
    end

    // ================= EX =================
    wire [31:0] write_data_alu;
wire [31:0] alu_operand2;
alu alu_unit (
    .src1(ID_EX_read_data1),
    .src2(alu_operand2),
    .alu_control(ID_EX_alu_control),
    .result(write_data_alu)
);


    assign beq  = (write_data_alu == 1 && ID_EX_beq_control);
    assign bneq = (write_data_alu == 1 && ID_EX_bne_control);
    assign bge  = (write_data_alu == 1 && ID_EX_bgeq_control);
    assign blt  = (write_data_alu == 1 && ID_EX_blt_control);
    assign alu_operand2 = (ID_EX_alu_src) 
                      ? ID_EX_imm_val
                      : ID_EX_read_data2;

    // ================= EX/MEM =================
    reg [31:0] EX_MEM_write_data_alu;
    reg [31:0] EX_MEM_data_out_2_dm;
    reg [4:0]  EX_MEM_write_reg_num1;
    reg        EX_MEM_sw, EX_MEM_lb, EX_MEM_mem_to_reg;
    reg EX_MEM_reg_write;
  reg [31:0] EX_MEM_pc_plus4;

    always @(posedge clk) begin
        if (rst) begin
            EX_MEM_write_data_alu <= 0;
            EX_MEM_data_out_2_dm <= 0;
            EX_MEM_write_reg_num1 <= 0;
            EX_MEM_sw <= 0;
            EX_MEM_lb <= 0;
            EX_MEM_mem_to_reg <= 0;
            EX_MEM_reg_write <= 0;
            EX_MEM_pc_plus4 <= 0;
        end else begin
            EX_MEM_write_data_alu <= write_data_alu;
            EX_MEM_data_out_2_dm <= ID_EX_read_data2;
            EX_MEM_write_reg_num1 <= ID_EX_write_reg_num1;
            EX_MEM_sw <= ID_EX_sw;
            EX_MEM_lb <= ID_EX_lb;
            EX_MEM_mem_to_reg <= ID_EX_mem_to_reg;
            EX_MEM_reg_write <= ID_EX_reg_write;
            EX_MEM_pc_plus4 <= ID_EX_pc_plus4;
        end
    end

    // ================= MEM =================
    wire [31:0] data_out;

    data_memory dmu (
        clk,
        rst,
        EX_MEM_write_data_alu[4:0],
        EX_MEM_data_out_2_dm,
        EX_MEM_sw,
        EX_MEM_write_data_alu[4:0],
        data_out
    );
     
      // ================= MEM/WB =================
    reg [4:0]  MEM_WB_write_reg_num1;
    wire [31:0] write_back_data;
    reg       MEM_WB_lb;
    reg       MEM_WB_sw;
    reg [31:0] MEM_WB_write_data_alu;
    reg [31:0] MEM_WB_data_out;
    reg        MEM_WB_mem_to_reg;
reg MEM_WB_reg_write;
reg [31:0] MEM_WB_pc_plus4;
    always @(posedge clk) begin
        if (rst) begin
            MEM_WB_write_data_alu <= 0;
            MEM_WB_data_out <= 0;
            MEM_WB_write_reg_num1 <= 0;
            MEM_WB_lb <= 0;
            MEM_WB_sw <= 0;
            MEM_WB_mem_to_reg <= 0;
             MEM_WB_reg_write <= 0;
             MEM_WB_pc_plus4 <= 0;
        end else begin
            MEM_WB_write_data_alu <= EX_MEM_write_data_alu;
            MEM_WB_data_out <= data_out;
            MEM_WB_write_reg_num1 <= EX_MEM_write_reg_num1;
            MEM_WB_lb <= EX_MEM_lb;
            MEM_WB_sw <= EX_MEM_sw;
            MEM_WB_mem_to_reg <= EX_MEM_mem_to_reg;
            MEM_WB_reg_write <= EX_MEM_reg_write;
            MEM_WB_pc_plus4 <= EX_MEM_pc_plus4;
        end
    end

    // ================= WB =================
assign write_back_data =
    jump ? MEM_WB_pc_plus4 :
    (MEM_WB_mem_to_reg ? MEM_WB_data_out : MEM_WB_write_data_alu);

    assign read_data_addr_dm = read_data_addr_dm_2;

endmodule

