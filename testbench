module top_tb;

    reg clk;
    reg reset;

    // ================= Instantiate DUT =================
    top_riscv dut (
        .clk(clk),
        .reset(reset)
    );

    // ================= Clock Generation =================
    initial begin
        clk = 0;
        forever #5 clk = ~clk;   // 10ns period
    end

    // ================= Reset Sequence =================
    initial begin
        reset = 1;
        #20;            // hold reset for 20ns
        reset = 0;
    end

    // ================= Waveform Dump =================
    initial begin
        $dumpfile("dump.vcd");
        $dumpvars(0, top_tb);

        // run long enough
        #2000;

        $display("Simulation finished.");
        $finish;
    end

    // ================= Debug Monitor =================
    always @(posedge clk) begin
        if (!reset) begin
            $display("Time=%0t | PC=%h | INSTR=%h",
                     $time,
                     dut.pc,
                     dut.instruction_out);
        end
    end

endmodule
