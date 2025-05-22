# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)
![Screenshot 2025-05-02 105633](https://github.com/user-attachments/assets/423b3571-cd7a-4229-ac65-632d47a5e5be)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)
```
reg clk;
reg reset;
reg up_down;
reg enable;
wire [3:0] count;


up_down_counter uut(
    .clk(clk),
    .reset(reset),
    .up_down(up_down),
    .enable(enable),
    .count(count)
);


initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end


initial begin

    reset = 1;
    up_down = 1;
    enable = 0;
    

    #20 reset = 0;
    

    #10 enable = 1;
    #200;  
    

    up_down = 0;
    #200;  
    

    enable = 0;
    #50;
    

    reset = 1;
    #20 reset = 0;
    

    #50;
    

    $finish;
end


initial begin
    $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
             $time, reset, up_down, enable, count);
end
◦ SDC (Synopsis Design Constraint) File (.sdc)
```
##Design
```always @(posedge clk or posedge reset) begin
    if (reset) 
    begin
        count <= 4'b0000;
    end

    else if (enable)
    begin
        if (up_down)
        begin
            count <= count + 1'b1;
        end
        else begin
            count <= count - 1'b1;
        end
    end
end
```
![Screenshot 2025-05-02 110344](https://github.com/user-attachments/assets/bda7bbb5-48cf-4634-bf74-a26529bfc6d1)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.
![WhatsApp Image 2025-05-22 at 21 06 57_d8a9ce13](https://github.com/user-attachments/assets/a61dbc69-d43c-496a-a739-10a956bd49b9)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:
![WhatsApp Image 2025-05-22 at 21 06 57_04ce3c4d](https://github.com/user-attachments/assets/94b2ba47-36a2-4a33-9bcc-8e1ab59ddd75)

#### Power Report:
![WhatsApp Image 2025-05-22 at 21 06 57_d83c5427](https://github.com/user-attachments/assets/4e1fd502-8c2d-4109-adac-02ff64c0fe3c)

#### Timing Report: 
![WhatsApp Image 2025-05-22 at 21 06 57_f3a44377](https://github.com/user-attachments/assets/8b2ca2c0-efef-4d23-855b-3ae75c61cc74)
![WhatsApp Image 2025-05-22 at 21 06 57_3bd12cc2](https://github.com/user-attachments/assets/a8f096d6-86df-426d-b25a-d8c6ca464ab8)

#### Result: 
![WhatsApp Image 2025-05-22 at 21 06 57_bbb698c9](https://github.com/user-attachments/assets/05c83cbd-2625-4bfd-bd77-c9f24c96762e)

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





