---
title : "Moving Average Filter"
category :
    - Filter
tag :
    - Filter
toc : true
toc_sticky: true
comments: true
sidebar_main: true
---

Moving Average Filter에 관한 포스팅입니다.

## Moving Average Filter
**Moving Average Filter**란 과거의 일부분부터 현재 시점까지의 평균을 계속 구해나가는 것이다.<br><br>
즉, 100개의 정보를 가지고 평균을 내겠다고 한다면, 현재 시점부터 이전 100개의 데이터를 가지고 평균을 내게 된다.<br>
(이때, 과거 정보의 개수는 지정되어야 한다.)<br><br>
n개의 데이터에 대한 이동 평균을 수식으로 표현하면 다음과 같다.<br><br>
$ \bar{X_{k}} = \frac{X_{k-n+1}+X_{k-n+2}+...+X_{k}}{n} $ <br>

### 장단점
일정 정보를 평균을 취하기 때문에 Noise에 강인하지만<br>
현재 시점에서 이전 정보들을 다 더하고 그 개수만큼 나누어 평균을 취해야 하기에
지연이 생길 수 있으며, 이때 실시간성의 보장에 의문이 생길 수 있다.<br><br>
측정 값의 Sequence(이전 정보의 개수)이 길어지고 값의 변동이 심해질수록 Moving Average Filter의 한계가 드러난다.

### MATLAB Code
```matlab
clc, clear
close all

%% Set Param
    % Set Simulation Time
        end_time = 5;
        delta_t = 0.001;
    % Set Sine Wave
        sine_mag1 = 3;
        sine_freq1 = 1;
        
        sine_mag2 = 0.5;
        sine_freq2 = 10;
    % M_avg
        MAF_SIZE = 100;

%% Simulation
n = 1;
for t = 0:delta_t:end_time
    y_normal = sine_mag1 * sin(sine_freq1*(2*pi*t));

    % White Noise
    y_white = sine_mag1 * sin(sine_freq1*(2*pi*t))...
       + sine_mag2 * sin(sine_freq2*(2*pi*t))...
       + 0.8 * randn(size(t));

    % get data
    sim_y_normal(n) = y_normal;
    sim_y_white(n) = y_white;

    % MVF
    sum = 0;
    if n > MAF_SIZE
        for k = n - MAF_SIZE:n
            sum = sum + sim_y_white(k);
        end
    end
    MVF(n) = sum/MAF_SIZE;

    sim_time(n) = t;
    n = n + 1;
end

%% Draw Graph
    figure('Units','pixels','pos',[100 500 800 600],'Color',[1, 1, 1]);

    subplot(3,1,1)
        Xmin =  0.0;  XTick = 1.0;  Xmax = end_time;
        Ymin = -3.0;  YTick = 1.0;  Ymax = 3.0;

        plot(sim_time, sim_y_normal, '-k', 'LineWidth', 2) % x축 데이터, y축 데이터, solid black
        legend('sine Wave');

        grid on;
        axis([Xmin Xmax Ymin Ymax])
        set(gca, 'XTick', [Xmin:XTick:Xmax]);
        set(gca, 'YTick', [Ymin:YTick:Ymax]);
    
        xlabel('Time (s)','FontSize',20);
        ylabel('Magnitude','FontSize',20);
        title('Normal Sine Wave','FontSize',25);

    subplot(3,1,2)
        Xmin =  0.0;  XTick = 1.0;  Xmax = end_time;
        Ymin = -3.0;  YTick = 1.0;  Ymax = 3.0;

        plot(sim_time, sim_y_white, '-k', 'LineWidth', 2) % x축 데이터, y축 데이터, solid black
        hold on;
        plot(sim_time, MVF, '-b', 'LineWidth',2) % Blue
        legend('sine noise', 'Moving Average Filter');

        grid on;
        axis([Xmin Xmax Ymin Ymax])
        set(gca, 'XTick', [Xmin:XTick:Xmax]);
        set(gca, 'YTick', [Ymin:YTick:Ymax]);
    
        xlabel('Time (s)','FontSize',20);
        ylabel('Magnitude','FontSize',20);
        title('White-Noise Sine Wave','FontSize',25);

    subplot(3,1,3)
        Xmin =  0.0;  XTick = 1.0;  Xmax = end_time;
        Ymin = -3.0;  YTick = 1.0;  Ymax = 3.0;

        plot(sim_time, MVF, '-b', 'LineWidth',2) % x축 데이터, y축 데이터, solid blue
        legend('Moving Average Filter');

        grid on;
        axis([Xmin Xmax Ymin Ymax])
        set(gca, 'XTick', [Xmin:XTick:Xmax]);
        set(gca, 'YTick', [Ymin:YTick:Ymax]);
    
        xlabel('Time (s)','FontSize',20);
        ylabel('Magnitude','FontSize',20);
        title('Filtered Graph','FontSize',25);
```

### Result
<p align="center"><img src="/MyPDF/MVF.png" width = "600" ></p>

📣<br>
본 포스팅의 언어 및 개발 환경 : `Matlab`, `Windows`  
포스팅에 대한 오류나 궁금한 점은 Comments를 작성해주시면, 많은 도움이 됩니다.💡
{: .notice--info}