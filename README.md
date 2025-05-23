# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)
![Screenshot 2025-05-02 105633](https://github.com/user-attachments/assets/e9ddadf6-a912-4210-b9c7-6a0dfc0a8f13)

VHDL Files (.v or .vhdl or .vhd)

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
◦ SDC (Synopsis Design Constraint) File (.sdc)
![Screenshot 2025-05-02 110344](https://github.com/user-attachments/assets/af7e0f82-53f4-4350-ad51-7e1419f679d7)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.
![Screenshot 2025-05-23 102444](https://github.com/user-attachments/assets/26d63e09-91a2-46e0-b3ae-4866311d8573)

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
![WhatsApp Image 2025-05-22 at 21 06 57_6841d05f](https://github.com/user-attachments/assets/2155a429-d1cf-4227-96af-6cdbc7ed1098)
![Screenshot 2025-05-23 101747](https://github.com/user-attachments/assets/80fe0477-adb3-480e-ba46-20ea366edeaf)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:
![WhatsApp Image 2025-05-22 at 21 06 57_64266050](https://github.com/user-attachments/assets/b701a1f2-cff1-4e4f-b9a0-e5a8d640210f)

#### Power Report:
![WhatsApp Image 2025-05-22 at 21 06 57_5d881ce4](https://github.com/user-attachments/assets/51bbacff-62f4-42ec-a636-7b5af9b9df02)

#### Timing Report: 
![WhatsApp Image 2025-05-22 at 21 06 57_9fe5a346](https://github.com/user-attachments/assets/74aa08f5-8075-4f08-926c-1da248eca46c)

#### Result: 
![WhatsApp Image 2025-05-22 at 21 06 57_10643b6e](https://github.com/user-attachments/assets/54cd829a-2894-438a-a85c-e37a8c0b4816)

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





