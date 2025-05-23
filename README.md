# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,
![Screenshot 2025-04-29 105748](https://github.com/user-attachments/assets/ecc89ab0-fb49-45f1-ae42-36fc044d186d)

◦ Liberty Files (.lib)

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
             $time, reset, up_down, enable, count);
end
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
◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.
![Screenshot 2025-05-23 102444](https://github.com/user-attachments/assets/12a398bd-81fc-40d0-bb34-c6cac33bed7a)

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
![WhatsApp Image 2025-05-22 at 21 06 57_b8adc1e8](https://github.com/user-attachments/assets/05b94d19-1617-4b63-bc2e-7bb68dd25462)
![Screenshot 2025-05-23 101747](https://github.com/user-attachments/assets/17856a91-2434-4472-a416-84573f7b3ff8)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:
![WhatsApp Image 2025-05-22 at 21 06 57_4e9105e2](https://github.com/user-attachments/assets/7cc587d6-115e-4fce-aee9-d02114ebc3a1)

#### Power Report:
![WhatsApp Image 2025-05-22 at 21 06 57_907eaeba](https://github.com/user-attachments/assets/0515ba4a-b760-41d4-85a2-b289bf87c09d)

#### Timing Report: 
![WhatsApp Image 2025-05-22 at 21 06 57_24732470](https://github.com/user-attachments/assets/121e25b2-4388-4a1d-88d9-6c2ffa2b1dc8)

#### Result: 
![WhatsApp Image 2025-05-22 at 21 06 57_9c3e55ae](https://github.com/user-attachments/assets/508cb8f3-80df-49b3-b811-30b138734cef)

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





