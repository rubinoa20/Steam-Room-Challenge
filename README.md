#include <kipr/botball.h>
int forward = -55;
int wire_forward = -55;
int f_speed = -95;
int line_speed = -18;
int g_line_speed = 50;
int backward = 15;
int g_back_speed = 55;
int l_motor = 0; 
int r_motor = 3;
int b_pause = 500;
int pause = 1500;
int black_pause = 3100;
int g_pause = 2800;
int infared_spot = 3000;
int water_spot = 3500;
int infared_spot_g = 280;
int s_average = 2750;
int f_average = 2000;
int v_speed = -90;
int infared = 5;
int range_front = 1;
int pick_up_b = 265;
int pick_up_g = 210;
int drop = 1700;
int drop_speed = 2500;
int servo = 0;
int s_pause = 2000;
int water = 2;
int main()
{
    wait_for_light(4);
    msleep(30000);
    shut_down_in(120.0);
    forwards_slow();
    msleep(2500);
    forwards_dos();
    msleep(1200);
    find_line();
    find_wall();
    setup_black();
    backwards_b();
    msleep(b_pause);
    wire_b();
    attatch_b_wire();
    backwards();
    drop_og();
    g_line_setup();
    g_back();
    find_wall_g();
    setup_g_wire();
    g_back_dos();
    wire_g();
    attatch_g_wire();
    backwards();
    drop_og();
    water_setup();
    water_go();
        
    printf("Hello World\n");
    return 0;
}

//Functions

void setup_black(){
    motor(r_motor,line_speed);
    msleep(black_pause);
    ao();
}
void forwards_line(){
    motor(l_motor,line_speed);
    motor(r_motor,line_speed);
}
void forwards(){
    motor(l_motor,forward);
    motor(r_motor,forward);
}
void forwards_slow(){
    motor(l_motor,-30);
    motor(r_motor,-30);
}
void forwards_dos(){
    motor(l_motor,-97);
    motor(r_motor,-97);
}
void backwards(){
 	motor(l_motor,backward);
    motor(r_motor,backward);
    msleep(pause);
    ao();
}
void backwards_b(){
 	motor(l_motor,backward);
    motor(r_motor,backward);
    msleep(1200);
    ao();
}
void g_back(){
 	motor(l_motor,g_back_speed);
    motor(r_motor,g_back_speed);
    msleep(1600);
    ao();
}
void g_back_dos(){
 	motor(l_motor,g_back_speed);
    motor(r_motor,g_back_speed);
    msleep(300);
    ao();
}
void right(){
 	motor(l_motor,forward);
    msleep(pause);
    ao();
}
void left(){
    motor(r_motor,forward);
    msleep(pause);
    ao();
}
void l_veer(){
    motor(l_motor,v_speed);
}
void r_veer(){
    motor(r_motor,v_speed);
}
void find_line(){
    while (analog(infared) < infared_spot){
    forwards();
    }
    ao();
    printf("found line!\n");
}
void find_wall(){
    while (analog(range_front) <= f_average){
     	forwards_line();
    if (analog(infared) < infared_spot){
        l_veer();
    }
    if (analog(infared) > infared_spot){
        r_veer();
    }
    
  }
    ao();
    printf("found wall!\n");
    printf("rangefinder = %d\n", analog(range_front));
}
void wire_b(){
    enable_servos();
    set_servo_position(servo,1300);
    msleep(s_pause);
    set_servo_position(servo,1000);
    msleep(s_pause);
    set_servo_position(servo, 700);
    msleep(s_pause);
	set_servo_position(servo,pick_up_b);
    msleep(s_pause);
    ao();
}
void wire_g(){
    enable_servos();
    set_servo_position(servo,1300);
    msleep(s_pause);
    set_servo_position(servo,1000);
    msleep(s_pause);
    set_servo_position(servo, 700);
    msleep(s_pause);
	set_servo_position(servo,245);
    msleep(s_pause);
    ao();
}
void drop_og(){
	enable_servos();
    set_servo_position(servo,700);
    msleep(s_pause);
    set_servo_position(servo, 1000);
    msleep(s_pause);
    set_servo_position(servo,1300);
    msleep(s_pause);
	set_servo_position(servo,drop);
    msleep(s_pause);
    ao();
}
void attatch_b_wire(){
    motor(r_motor, -58);
    motor(l_motor, -24);
    msleep(1100);
    ao();
}
void find_wall_g(){
    while (analog(range_front) <= f_average){
     	forwards_line();
    if (analog(infared) > infared_spot_g){
        l_veer();
    }
    if (analog(infared) < infared_spot_g){
        r_veer();
    }
    
  }
    ao();
    printf("found wall!\n");
    printf("rangefinder = %d\n", analog(range_front));
}
void g_line_setup(){
	motor(r_motor,g_back_speed);
    msleep(1750);
    ao();
}
void setup_g_wire(){
    motor(l_motor,line_speed);
    msleep(1800);
    ao();
} 
void attatch_g_wire(){
    motor(l_motor,-60);
    motor(r_motor, -9);
    msleep(800);
    ao();
}
void water_setup(){
	motor(l_motor,55);
    motor(r_motor,-55);
    msleep(2800);
    ao();
    msleep(500);
}
void find_water(){
    while (analog(range_front) <= f_average){
     	forwards();
        if (analog(range_front) >= f_average){
            ao();
        }
}
}
void f_turn(){
    motor(l_motor,f_speed);
    motor(r_motor,forward);
}
void s_turn(){
    motor(l_motor,forward);
    motor(r_motor,f_speed);
}
void water_go(){
    find_water();
    motor(r_motor,-20);
    motor(l_motor,-20);
    msleep(850);
    ao();
    motor(r_motor,-60);
    motor(l_motor,-30);
    msleep(1700);
    ao();
    forwards();
    msleep(9500);
    ao();
}
