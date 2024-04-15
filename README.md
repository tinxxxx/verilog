# verilog
Addition, subtraction, multiplication and division
module mm(a, b, op, sum,seg_a,seg_b,seg_t,seg_o,seg_r, sign_first,sign_second );
	input  [3:0] a, b;
	input [1:0] op;
	output sign_first,sign_second;
	output signed [7:0] sum;
	output [7:0] seg_a,seg_b,seg_t,seg_o,seg_r;
	reg [7:0] seg_a,seg_b,seg_t,seg_o,seg_r,sum,sum1,mod,mod1;
	reg sign_first,sign_second;
	wire [3:0] t, o;
	
	always@(op) begin
		if(op == 2'b00) begin
			sum=a+b;
			sign_first=0;
			sign_second=0;
		end
		else if(op == 2'b01) begin
			sum=a-b;
			sign_first=0;
			sign_second=1;
		end
		else if(op == 2'b10) begin
			sum=a*b;
			sign_first=1;
			sign_second=0;
		end
		else begin
			sum=a/b;
			mod=a%b;
			sign_first=1;
			sign_second=1;
		end
	end
	
	always@(sum or t or a or b or op) begin
		if(op==2'b01 && a<b) begin
			seg_t = 8'b10111111;
			sum1 = ~(sum) + 1;
		end
		
		else begin
			sum1 = sum;
			case(t)
			4'b0000 : seg_t = 8'b11000000;//0
			4'b0001 : seg_t = 8'b11111001;//1
			4'b0010 : seg_t = 8'b10100100;//2
			4'b0011 : seg_t = 8'b10110000;//3
			4'b0100 : seg_t = 8'b10011001;//4
			4'b0101 : seg_t = 8'b10010010;//5
			4'b0110 : seg_t = 8'b10000010;//6
			4'b0111 : seg_t = 8'b11111000;//7
			4'b1000 : seg_t = 8'b10000000;//8
			4'b1001 : seg_t = 8'b10011000;//9
			endcase
		end
		
		
	end
	
	assign t = sum1 / 10;
	assign o = sum1 % 10;
	
	
	
	always@(o) begin
		case(o)
			4'b0000 : seg_o = 8'b11000000;//0
			4'b0001 : seg_o = 8'b11111001;//1
			4'b0010 : seg_o = 8'b10100100;//2
			4'b0011 : seg_o = 8'b10110000;//3
			4'b0100 : seg_o = 8'b10011001;//4
			4'b0101 : seg_o = 8'b10010010;//5
			4'b0110 : seg_o = 8'b10000010;//6
			4'b0111 : seg_o = 8'b11111000;//7
			4'b1000 : seg_o = 8'b10000000;//8
			4'b1001 : seg_o = 8'b10011000;//9
		endcase
	end
	
	always@(mod or op) begin
		if(mod[7] == 0 && op == 2'b11) begin
			mod1 = mod;
		end
	end
	
	
	always@(mod1) begin
		if(op!=2'b11)begin
			seg_r = 8'b11111111;
		end
		else begin 
		case(mod1)
			7'b0000000 : seg_r = 8'b11000000;//0
			7'b0000001 : seg_r = 8'b11111001;//1
			7'b0000010 : seg_r = 8'b10100100;//2
			7'b0000011 : seg_r = 8'b10110000;//3
			7'b0000100 : seg_r = 8'b10011001;//4
			7'b0000101 : seg_r = 8'b10010010;//5
			7'b0000110 : seg_r = 8'b10000010;//6
			7'b0000111 : seg_r = 8'b11111000;//7
			7'b0001000 : seg_r = 8'b10000000;//8
			7'b0001001 : seg_r = 8'b10011000;//9
			
		endcase
	end
	end
	
	always@(a)begin
		case(a)
			4'b0000:	seg_a = 8'b11000000;//0
			4'b0001:	seg_a = 8'b11111001;//1
			4'b0010:	seg_a = 8'b10100100;//2
			4'b0011:	seg_a = 8'b10110000;//3
			4'b0100:	seg_a = 8'b10011001;//4
			4'b0101:	seg_a = 8'b10010010;//5
			4'b0110:	seg_a = 8'b10000010;//6
			4'b0111:	seg_a = 8'b11011000;//7
			4'b1000:	seg_a = 8'b10000000;//8
			4'b1001: seg_a = 8'b10011000;//9
			default:	seg_a = 8'b11111111;//全暗
		endcase
	end
	
	always@(b)begin
		case(b)
			4'b0000:	seg_b = 8'b11000000;//0
			4'b0001:	seg_b = 8'b11111001;//1
			4'b0010:	seg_b = 8'b10100100;//2
			4'b0011:	seg_b = 8'b10110000;//3
			4'b0100:	seg_b = 8'b10011001;//4
			4'b0101:	seg_b = 8'b10010010;//5
			4'b0110:	seg_b = 8'b10000010;//6
			4'b0111:	seg_b = 8'b11011000;//7
			4'b1000:	seg_b = 8'b10000000;//8
			4'b1001: seg_b = 8'b10011000;//9
			default:	seg_b = 8'b11111111;//全暗
		endcase
		end
endmodule
