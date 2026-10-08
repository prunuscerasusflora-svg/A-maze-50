# A-maze-50
A game made by Julie Vitali, Léna Hugober and Cerise Grémont
%{
===============================================================================
  PROGRAM NAME: amaze_game_VF.m
  
  GOAL OF THE GAME:
 The goal of the game is to reach the end of the labyrinth before the allotted time so as not to get eaten by spiders. 

COMPONENTS:
 a maze → 3 corridors/level :dead end, trapped in a spider’s web, good one
 a mouse → our character
 hallway decoration: torches,clouds
 game over  :  spider’s web, spiders
 win end : outdoor landscape
 differents wood panels :
   - choice of the direction → arrows : left, middle, right
   -validation or return → green button (confirm the choice) , red button (cancel the choice) 
   -choice of the difficulty→ [1]debutant , [2]intermediate, [3]expert
   -menu → G(start the game), P (time-out), S(stop the game, return to the menu panel),           R(replay)
   -choice to return in the menu at the end of the game → enter (return to the menu panel)
   -contect panel → enter (go to the menu panel)

%MAIN LOOP:
% when the direction sign is displayed
   case 'PLAY_CHOICE' 
%if our cursor is positioned on the left-hand arrow button corresponding to the next set of coordinates on the screen
            if x >= 20 && x <= 38 && y >= 36 && y <= 54
%and when you click at that point on the screen, the confirmation panel appears and the timer pauses
                state.pending_choice = 1; state.mode = 'CONFIRM_CHOICE'; state.pause_start 
%to record the exact time at which the confirmation screen opens, so as to count down the time for confirmation once the player has clicked ‘Confirm’
= time();
%otherwise our cursor is positioned on the middle-hand arrow button corresponding to the next set of coordinates on the screen
         elseif x >= 41 && x <= 59 && y >= 36 && y <= 54
%and when you click at that point on the screen, the confirmation panel appears and the timer pauses
             state.pending_choice = 2; state.mode = 'CONFIRM_CHOICE'; state.pause_start 
%to record the exact time at which the confirmation screen opens, so as to count down the time for confirmation once the player has clicked ‘Confirm’
= time();
%otherwise our cursor is positioned on the right-hand arrow button corresponding to the next set of coordinates on the screen
           elseif x >= 62 && x <= 80 && y >= 36 && y <= 54
%and when you click at that point on the screen, the confirmation panel appears and the timer pauses
               state.pending_choice = 3; state.mode = 'CONFIRM_CHOICE'; state.pause_start 
%to record the exact time at which the confirmation screen opens, so as to count down the time for confirmation once the player has clicked ‘Confirm’
= time();
%end of the loop
         end

%RULES:
  -Press G to start playing 
  -select the difficulty level
  -select a path using the arrows on the screen
  - Confirm your choice or return to the previous screen
  -If you’ve chosen the right path, move on to the next level 
  - finds their way out of the maze before the timer runs out
  -If you want to stop playing, press S
  -If you want to pause, press P


WAY TO MOVE:
Click on one of the arrows on the screen and confirm your choice; the mouse will move accordingly.

CONTEXT:
Pip the mouse has strumbled into the Queen Muzet’s lair ! 
 Spiders are lurking behind every wall
To reach the surface of the daylight, Pip must traverse ten winding gates inside the underground capital.
Choose wisely: Dead ends and webs will stall Pip and Queen  Muzet’s army is closing in!
  

SOURCE OF IA/ SOUND/IMAGE:
%
IA source → Gemini
sounds → no music
images → Images created using gemini, no imported images

OCTAVE AND CODE VERSION
octave version → octave-11.3.0-w64-installer.exe 
code version → 50

AUTHORS AND CONTRIBUTIONS:
discussion on the concept of the game→  Cerise Grémont, Léna Hugober, Julie Vitali
drafting the first draft → Cerise Grémont, Léna Hugober, Julie Vitali
detailed prompt drafting → Léna Hugober, Julie Vitali
scriptwriting →  Julie Vitali
comments on the lines of the script→ Cerise Grémont, Léna Hugober
resolving issues in the script →Julie Vitali
writing the header→ Léna Hugober,Cerise Grémont

DATE:
08/10/2026 (day/month/year)


VARIABLES:
Main Figure & Global State Structure (state)
hFig: Handle to the main game figure window.
state: Structure containing all game data, modes, settings, and animation variables.
state.mode: Current screen/game state (e.g., 'STORY', 'MENU', 'DIFFICULTY', 'PLAY_CHOICE', 'PAUSE', 'WIN', 'GAMEOVER', etc.).
state.current_level: Current level number (from 1 to 10).
state.max_levels: Maximum number of levels required to win (10).
state.time_limit: Total time limit allocated based on the chosen difficulty (in seconds).
state.total_paused_time: Cumulative duration spent in pauses or scene animations.
state.pause_start: Timestamp marking when a pause or confirmation screen began.
state.start_time: Timestamp marking when the game session started.
state.penalty: Time penalty accumulated (e.g., +5 seconds from spider web traps).
state.paliers: Matrix storing randomized path outcomes (1-3) for each level.
state.pending_choice: Player's selected direction waiting for confirmation (1: Left, 2: Up, 3: Right).
state.mouse_pos: Coordinates [x, y] representing Pip the mouse's position.
state.tunnel_zoom: Zoom progression factor during tunnel transition animations.
state.frame_time: Time accumulator used to drive animations, flickering lights, and movement.
state.web_progress: Completion percentage of escaping a spider web trap.
state.cloud_x: Horizontal coordinates for background pixel clouds.
state.confetti_x / confetti_y: Coordinates for win-screen confetti particles.
state.confetti_vy: Vertical velocities for confetti particles.
state.confetti_colors: RGB colors assigned to confetti particles.
Event & Callback Variables
ax: Handle to the active axes container.
cp: Current mouse pointer coordinates inside the axes.
x / y: Horizontal and vertical coordinates for clicks, layout bounds, or drawing elements.
src: Handle of the object (figure) triggering callbacks.
event: Event structure containing details for keyboard inputs.
key: Lowercase string representing the key pressed by the user.
time_limit_sec: Time limit passed when initializing a new game.
choice: Index representing the player's chosen path direction.
Animation & Scene Loop Variables
target_x / target_y: Target coordinate arrays for mouse movement animations.
start_x / start_y: Initial starting coordinates for mouse movement animations.
steps: Number of interpolation steps in movement animations.
t_anim_start: Start timestamp of movement or choice sequences.
k: Loop iteration counter.
t: Normalized interpolation parameter (0 to 1) or time vector for drawing curves.
st_check: Temporary state snapshot checked within loops to handle sudden menu interruptions.
path_type: Outcome type of the chosen path (1: Tunnel, 2: Dead End, 3: Web Trap).
t_dead_start: Start timestamp for the dead-end scene.
duration: Set duration length for specific scene displays.
t_web_start: Start timestamp for the spider web trap scene.
elapsed: Total elapsed game or trap time calculated dynamically.
t_start: Start timestamp for tunnel transition animations.
z: Current zoom step value during the tunnel sequence.
UI, Layout, & Rendering Variables
box_w / box_h: Width and height dimensions of the Undertale battle box.
box_x / box_y: Bottom-left corner coordinates of the battle box.
idx: Loop index for iterating through story text lines.
story_text: Cell array containing the introductory story paragraphs.
remaining: Time remaining before game over.
mins / secs: Calculated minutes and seconds left on the clock.
names: Cell array holding direction labels ({'LEFT', 'UP', 'RIGHT'}).
bw / bh: Local reference copies of box width and height used in tunnel rendering.
t_shift: Positional shift offset for animated torches.
mouse_size: Scaling factor for drawing the mouse at different depths/distances.
mouse_y_pos: Calculated vertical position of the sprinting mouse in the tunnel.
reset_idx: Logical index array for recycling off-screen confetti particles.
i: General iterator for loops.
Drawing Helper Variables
t_swarm: Time-scaling factor for spider swarm movement in the Game Over screen.
s1_x to s4_x / s1_y to s4_y: Coordinates for individual spiders in the Game Over swarm.
scale: Multiplier used to scale pixel art elements.
purple_core / purple_border / dark_purple: RGB color vectors for aesthetic path styling.
flicker: Sine-based offset for torch flame flickering effects.
r: Radius or loop variable for torches and web rings.
alpha_val: Opacity/blending factor for torch glow gradients.
txt: String label displayed inside button components.
leg: Multiplier/index for rendering spider legs symmetrically.
purple_dark / purple_mid: RGB color vectors for castle background layers.
px / py: Pixel coordinates for castle towers.
cx / cy: Center coordinates for drawing centered boxes.
w / h: Width and height parameters for generic drawing helpers.
c_col: Color vector for pixel clouds.
radius: Radius parameter for generating spider web geometry.
web_color: RGB color vector for drawing web lines.

===============================================================================
%}

function amaze_game_VF()
    close all;

    % Force le toolkit graphique sous Octave pour s'assurer que la fenêtre s'ouvre
    if exist('OCTAVE_VERSION', 'builtin')
        graphics_toolkit('qt');
    end

    hFig = figure('Name', 'A-Maze! - Halloween Edition', ...
                  'NumberTitle', 'off', ...
                  'MenuBar', 'none', 'ToolBar', 'none', ...
                  'Color', [0.03, 0.01, 0.05], ...
                  'Units', 'normalized', ...
                  'Position', [0.1 0.1 0.8 0.8], ...
                  'KeyPressFcn', @on_keypress, ...
                  'WindowButtonDownFcn', @on_mouseclick);

    state = struct();
    state.mode = 'STORY';
    state.current_level = 1;
    state.max_levels = 10;
    state.time_limit = 300;
    state.total_paused_time = 0;
    state.pause_start = 0;
    state.start_time = 0;
    state.penalty = 0;
    state.paliers = [];
    state.pending_choice = 0;
    state.mouse_pos = [50, 10];
    state.tunnel_zoom = 0;
    state.frame_time = 0;
    state.web_progress = 0;

    state.cloud_x = [10, 48, 82];

    rng('shuffle');

    % Win Confetti
    state.confetti_x = rand(1, 50) * 100;
    state.confetti_y = rand(1, 50) * 100 + 100;
    state.confetti_vy = rand(1, 50) * 3 + 2;
    state.confetti_colors = rand(50, 3);

    setappdata(hFig, 'state', state);
    render_game(hFig);

    % Main render loop (~30 FPS)
    while ishandle(hFig)
        state = getappdata(hFig, 'state');
        state.frame_time = state.frame_time + 0.2;
        state.cloud_x = mod(state.cloud_x + 0.25, 120) - 10;

        setappdata(hFig, 'state', state);
        render_game(hFig);
        pause(0.02);
    end
endfunction
%%Serves as the main entry point of the script, initializing the game figure window, setting up the global state structure, and driving the primary render loop (~30 FPS).%

% --- Mouse Click Event —
function on_mouseclick(src, ~)
    state = getappdata(src, 'state');
    ax = gca();
    cp = get(ax, 'CurrentPoint');
    x = cp(1,1); y = cp(1,2);

    switch state.mode
        case 'PLAY_CHOICE'
            if x >= 15 && x <= 35 && y >= 2 && y <= 10
                state.pending_choice = 1; state.mode = 'CONFIRM_CHOICE'; state.pause_start = time();
            elseif x >= 40 && x <= 60 && y >= 2 && y <= 10
                state.pending_choice = 2; state.mode = 'CONFIRM_CHOICE'; state.pause_start = time();
            elseif x >= 65 && x <= 85 && y >= 2 && y <= 10
                state.pending_choice = 3; state.mode = 'CONFIRM_CHOICE'; state.pause_start = time();
            end
        case 'CONFIRM_CHOICE'
            if x >= 25 && x <= 75 && y >= 24 && y <= 32
                state.total_paused_time = state.total_paused_time + (time() - state.pause_start);
                setappdata(src, 'state', state);
                run_choice_sequence(src, state.pending_choice);
                return;
            elseif x >= 25 && x <= 75 && y >= 12 && y <= 20
                state.total_paused_time = state.total_paused_time + (time() - state.pause_start);
                state.mode = 'PLAY_CHOICE';
            end
    end
    setappdata(src, 'state', state);
endfunction
%Handles mouse click events within the window. It checks the click coordinates during choice screens and confirmation menus to process the player's navigation choices.%

% --- Keyboard Event ---
function on_keypress(src, event)
    state = getappdata(src, 'state');
    key = lower(event.Key);

    if strcmp(key, 's')
        state.mode = 'MENU';
        setappdata(src, 'state', state);
        render_game(src);
        return;
    end

    if strcmp(key, 'r')
        if ~strcmp(state.mode, 'STORY') && ~strcmp(state.mode, 'MENU') && ~strcmp(state.mode, 'DIFFICULTY')
            start_new_game(src, state.time_limit);
            return;
        end
    end

    switch state.mode
        case 'STORY'
            if strcmp(key, 'return'), state.mode = 'MENU'; end
        case 'MENU'
            if strcmp(key, 'g'), state.mode = 'DIFFICULTY'; end
        case 'DIFFICULTY'
            if strcmp(key, '1') || strcmp(key, 'num_1')
                start_new_game(src, 300); return;
            elseif strcmp(key, '2') || strcmp(key, 'num_2')
                start_new_game(src, 180); return;
            elseif strcmp(key, '3') || strcmp(key, 'num_3')
                start_new_game(src, 90);  return;
            end
        case 'PLAY_CHOICE'
            if strcmp(key, 'p')
                state.mode = 'PAUSE'; state.pause_start = time();
            end
        case 'CONFIRM_CHOICE'
            if strcmp(key, 'return')
                state.total_paused_time = state.total_paused_time + (time() - state.pause_start);
                setappdata(src, 'state', state);
                run_choice_sequence(src, state.pending_choice);
                return;
            end
        case 'PAUSE'
            if strcmp(key, 'p')
                state.total_paused_time = state.total_paused_time + (time() - state.pause_start);
                state.mode = 'PLAY_CHOICE';
            end
        case {'GAMEOVER', 'WIN'}
            if strcmp(key, 'return'), state.mode = 'DIFFICULTY'; end
    end
    setappdata(src, 'state', state);
endfunction
%Manages keyboard inputs from the player. It allows switching between the story, menu, and difficulty screens, handles game pauses, and lets the player restart.%

function start_new_game(fig, time_limit_sec) %Resets and initializes core gameplay variables (such as timers, the level counter, and penalties) when beginning a new session or restarting.%
    state = getappdata(fig, 'state');
    state.time_limit = time_limit_sec;
    state.current_level = 1;
    state.penalty = 0;
    state.total_paused_time = 0;
    state.start_time = time();
    state.mode = 'PLAY_CHOICE';
    state.mouse_pos = [50, 12];

    state.paliers = zeros(10, 3);
    for i = 1:10, state.paliers(i, :) = randperm(3); end

    setappdata(fig, 'state', state);
endfunction
%Resets and initializes the game variables for a fresh playthrough. It sets the chosen time limit, resets the level counter to 1, and generates a new randomized path outcome matrix (paliers).%

function run_choice_sequence(fig, choice)
    state = getappdata(fig, 'state');
    state.mode = 'ANIMATING';

    target_x = [23, 50, 77];
    target_y = [34, 37, 34];
    start_x = state.mouse_pos(1);
    start_y = state.mouse_pos(2);

    steps = 2;
    t_anim_start = time();
    for k = 1:steps
        if ~ishandle(fig), return; end
        st_check = getappdata(fig, 'state');
        if strcmp(st_check.mode, 'MENU'), return; end

        t = k / steps;
        state.mouse_pos(1) = start_x + (target_x(choice) - start_x) * t;
        state.mouse_pos(2) = start_y + (target_y(choice) - start_y) * t;

        state.frame_time = state.frame_time + 0.2;
        state.cloud_x = mod(state.cloud_x + 0.25, 120) - 10;

        setappdata(fig, 'state', state);
        render_game(fig);
        pause(0.005);
    end
    state.total_paused_time = state.total_paused_time + (time() - t_anim_start);
    setappdata(fig, 'state', state);

    path_type = state.paliers(state.current_level, choice);

    if path_type == 1 % Path (Tunnel transition)
        run_tunnel_animation(fig);
        st_check = getappdata(fig, 'state');
        if strcmp(st_check.mode, 'MENU'), return; end

        state.current_level = state.current_level + 1;
        state.mouse_pos = [50, 12];
        if state.current_level > state.max_levels
            trigger_win(fig);
        else
            state.mode = 'PLAY_CHOICE';
            setappdata(fig, 'state', state);
        end
    elseif path_type == 2 % Dead End
        state.mode = 'SCENE_DEADEND';
        t_dead_start = time();
        duration = 1.8;

        while (time() - t_dead_start) < duration
            if ~ishandle(fig), return; end
            st_check = getappdata(fig, 'state');
            if strcmp(st_check.mode, 'MENU'), return; end

            state.frame_time = state.frame_time + 0.2;
            state.cloud_x = mod(state.cloud_x + 0.25, 120) - 10;

            setappdata(fig, 'state', state);
            render_game(fig);
            pause(0.02);
        end

        state.total_paused_time = state.total_paused_time + duration;
        st_check = getappdata(fig, 'state');
        if strcmp(st_check.mode, 'MENU'), return; end

        state.mouse_pos = [50, 12];
        state.mode = 'PLAY_CHOICE';
        setappdata(fig, 'state', state);

    elseif path_type == 3 % Spider Web Trap
        state.penalty = state.penalty + 5;
        state.mode = 'SCENE_WEB';
        state.mouse_pos = [50, 28];

        t_web_start = time();
        duration = 2.5;
        while (time() - t_web_start) < duration
            if ~ishandle(fig), return; end
            st_check = getappdata(fig, 'state');
            if strcmp(st_check.mode, 'MENU'), return; end

            elapsed = time() - t_web_start;
            state.web_progress = (elapsed / duration) * 100;

            state.mouse_pos(1) = 50 + sin(elapsed * 40) * 0.8;
            state.mouse_pos(2) = 28 + cos(elapsed * 35) * 0.5;

            state.frame_time = state.frame_time + 0.2;
            state.cloud_x = mod(state.cloud_x + 0.25, 120) - 10;

            setappdata(fig, 'state', state);
            render_game(fig);
            pause(0.02);
        end
        state.total_paused_time = state.total_paused_time + duration;

        run_tunnel_animation(fig);
        st_check = getappdata(fig, 'state');
        if strcmp(st_check.mode, 'MENU'), return; end

        state.current_level = state.current_level + 1;
        state.mouse_pos = [50, 12];
        if state.current_level > state.max_levels
            trigger_win(fig);
        else
            state.mode = 'PLAY_CHOICE';
            setappdata(fig, 'state', state);
        end
    end
endfunction
%Manages the animation and outcome of a player's path choice. It animates Pip the mouse moving toward the selected direction and determines whether the path leads to a tunnel, a dead end, or a spider web trap.%

function run_tunnel_animation(fig)
    state = getappdata(fig, 'state');
    state.mode = 'SCENE_TUNNEL';
    t_start = time();

    for z = 0:0.1:1.0
        if ~ishandle(fig), return; end
        st_check = getappdata(fig, 'state');
        if strcmp(st_check.mode, 'MENU'), return; end

        state.tunnel_zoom = z;
        state.frame_time = state.frame_time + 0.3;
        state.cloud_x = mod(state.cloud_x + 0.25, 120) - 10;

        setappdata(fig, 'state', state);
        render_game(fig);
        pause(0.02);
    end
    state.total_paused_time = state.total_paused_time + (time() - t_start);
    setappdata(fig, 'state', state);
endfunction
%Handles the visual transition when Pip successfully navigates through a tunnel. It dynamically increments the zoom level to simulate moving deeper into the labyrinth.%

function trigger_win(fig)
    state = getappdata(fig, 'state');
    state.mode = 'WIN';
    setappdata(fig, 'state', state);
endfunction
%Switches the game state to 'WIN' when the player completes all levels. This signals the rendering engine to display the victory screen and confetti particles.%

% --- Main Render Engine ---
function render_game(fig)
    if ~ishandle(fig), return; end
    state = getappdata(fig, 'state');

    ax = findobj(fig, 'type', 'axes');
    if isempty(ax)
        ax = axes('Parent', fig, 'Position', [0 0 1 1]);
    else
        cla(ax);
    end

    hold(ax, 'on');
    axis(ax, [0 100 0 100]);
    axis(ax, 'off');

    if strcmp(state.mode, 'WIN')
        rectangle('Position', [0 0 100 100], 'FaceColor', [0.2, 0.5, 0.8], 'EdgeColor', 'none');
        draw_surface_scenery();
    else
        rectangle('Position', [0 0 100 100], 'FaceColor', [0.03, 0.01, 0.06], 'EdgeColor', 'none');
        draw_pixelated_castle_scenery();
        draw_subtle_torch(8, 55, state.frame_time);
        draw_subtle_torch(92, 55, state.frame_time + 0.5);
    end

    for i = 1:3
        draw_pixel_cloud(state.cloud_x(i), 68 + mod(i, 2)*4, 0.8);
    end

    box_w = 82; box_h = 38;
    box_x = 50 - box_w/2; box_y = 28 - box_h/2;

    switch state.mode
        case 'STORY'
            draw_undertale_battle_box(50, 50, 80, 68);
            text(50, 78, '🎃 PIP''S ESCAPE 🎃', 'FontSize', 22, 'FontWeight', 'bold', 'Color', [1 0.7 0.2], 'HorizontalAlignment', 'center');

            story_text = {
                'Pip the Mouse has stumbled into Queen Muzet''s lair!';
                'Spiders are lurking behind every wall.';
                '';
                'To reach the surface daylight, Pip must traverse 10';
                'winding gates inside the Underground Capital.';
                '';
                'Choose wisely: Dead ends and webs will stall Pip,';
                'and Queen Muzet''s army is closing in!';
            };
            for idx = 1:length(story_text)
                text(50, 71 - idx*4.5, story_text{idx}, 'FontSize', 11, 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            end

            text(50, 22, '[ Press Enter to Start ]', 'FontSize', 14, 'FontWeight', 'bold', 'Color', [0.4 1 0.5], 'HorizontalAlignment', 'center');

        case 'MENU'
            draw_undertale_battle_box(50, 48, 72, 58);
            text(50, 70, 'A-MAZE!', 'FontSize', 28, 'FontWeight', 'bold', 'Color', [1 0.5 0.2], 'HorizontalAlignment', 'center');
            text(50, 60, '🎃 HALLOWEEN ESCAPE 🎃', 'FontSize', 11, 'FontWeight', 'bold', 'Color', [1 0.8 0.4], 'HorizontalAlignment', 'center');
            text(50, 48, 'Help Pip escape through 10 levels using choices.', 'FontSize', 11, 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            text(50, 40, 'P: Pause | R: Replay | S: Story / Menu', 'FontSize', 10, 'Color', [0.8 0.8 0.8], 'HorizontalAlignment', 'center');
            text(50, 26, 'Press "G" to Choose Difficulty', 'FontSize', 16, 'FontWeight', 'bold', 'Color', [0.4 1 0.5], 'HorizontalAlignment', 'center');

        case 'DIFFICULTY'
            draw_undertale_battle_box(50, 48, 70, 56);
            text(50, 68, 'SELECT DIFFICULTY', 'FontSize', 20, 'FontWeight', 'bold', 'Color', [1 0.6 0.2], 'HorizontalAlignment', 'center');
            text(50, 52, '[1] Beginner : 5 min', 'FontSize', 15, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            text(50, 40, '[2] Medium   : 3 min', 'FontSize', 15, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            text(50, 28, '[3] Expert   : 1 min 30s', 'FontSize', 15, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');

        case {'PLAY_CHOICE', 'CONFIRM_CHOICE', 'PAUSE', 'ANIMATING'}
            if strcmp(state.mode, 'PAUSE') || strcmp(state.mode, 'CONFIRM_CHOICE')
                elapsed = ((time() - state.start_time) - state.total_paused_time - (time() - state.pause_start)) + state.penalty;
            else
                elapsed = ((time() - state.start_time) - state.total_paused_time) + state.penalty;
            end

            remaining = state.time_limit - elapsed;
            if remaining <= 0
                state.mode = 'GAMEOVER';
                setappdata(fig, 'state', state);
                return;
            end

            mins = max(0, floor(remaining / 60)); secs = max(0, floor(mod(remaining, 60)));
            rectangle('Position', [2 88 38 10], 'FaceColor', [0.08 0.04 0.12], 'EdgeColor', [1 0.6 0.2], 'LineWidth', 2);
            text(20, 93, sprintf('LEVEL: %d / 10', state.current_level), 'FontSize', 16, 'FontWeight', 'bold', 'Color', [0.4 1 0.9], 'HorizontalAlignment', 'center');

            rectangle('Position', [60 88 38 10], 'FaceColor', [0.08 0.04 0.12], 'EdgeColor', [1 0.6 0.2], 'LineWidth', 2);
            text(80, 93, sprintf('TIME: %02d:%02d', mins, secs), 'FontSize', 16, 'FontWeight', 'bold', 'Color', [1 0.9 0.2], 'HorizontalAlignment', 'center');

            draw_undertale_battle_box(50, 28, box_w, box_h);
            draw_unified_aesthetic_paths(box_x, box_y, box_w, box_h);
            draw_halloween_decorations(box_x, box_y, box_w, box_h);

            draw_pixel_mouse(state.mouse_pos(1), state.mouse_pos(2), 1.5);

            if strcmp(state.mode, 'PLAY_CHOICE')
                draw_undertale_battle_box(50, 5, 82, 8);
                draw_arrow_button(25, 5, '< LEFT');
                draw_arrow_button(50, 5, '^ UP ^');
                draw_arrow_button(75, 5, 'RIGHT >');

            elseif strcmp(state.mode, 'CONFIRM_CHOICE')
                names = {'LEFT', 'UP', 'RIGHT'};
                text(50, 38, sprintf('* Go %s?', names{state.pending_choice}), 'FontSize', 13, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');

                rectangle('Position', [28 24 44 8], 'FaceColor', [0.1 0.6 0.2], 'EdgeColor', [1 1 1], 'LineWidth', 2);
                text(50, 28, 'YES', 'FontSize', 12, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');

                rectangle('Position', [32 12 36 6], 'FaceColor', [0.6 0.1 0.1], 'EdgeColor', [1 1 1]);
                text(50, 15, 'NO', 'FontSize', 10, 'FontWeight', 'bold', 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            end

            if strcmp(state.mode, 'PAUSE')
                draw_undertale_battle_box(50, 28, 40, 14);
                text(50, 28, 'PAUSED', 'FontSize', 15, 'FontWeight', 'bold', 'Color', [1 0.9 0.2], 'HorizontalAlignment', 'center');
            end

        case 'SCENE_TUNNEL'
            bw = box_w;
            bh = box_h;

            draw_undertale_battle_box(50, 28, bw, bh);
            z = state.tunnel_zoom;

            fill([box_x, box_x+25, box_x+25, box_x], [box_y, box_y+10, box_y+bh-10, box_y+bh], [0.12 0.05 0.18], 'EdgeColor', [0.4 0.2 0.5], 'LineWidth', 2);
            fill([box_x+bw, box_x+bw-25, box_x+bw-25, box_x+bw], [box_y, box_y+10, box_y+bh-10, box_y+bh], [0.12 0.05 0.18], 'EdgeColor', [0.4 0.2 0.5], 'LineWidth', 2);
            fill([box_x, box_x+bw, box_x+bw-25, box_x+25], [box_y+bh, box_y+bh, box_y+bh-10, box_y+bh-10], [0.18 0.08 0.25], 'EdgeColor', 'none');
            fill([box_x, box_x+bw, box_x+bw-25, box_x+25], [box_y, box_y, box_y+10, box_y+10], [0.08 0.03 0.12], 'EdgeColor', 'none');

            rectangle('Position', [50-12, 28-8, 24, 16], 'FaceColor', [1 0.85 0.4], 'EdgeColor', [1 1 1], 'LineWidth', 2);
            rectangle('Position', [50-9, 28-6, 18, 12], 'FaceColor', [1 1 0.8], 'EdgeColor', 'none');

            t_shift = mod(state.frame_time * 3, 10);
            draw_subtle_torch(box_x + 8 + t_shift*0.8, box_y + 18, state.frame_time);
            draw_subtle_torch(box_x + bw - 8 - t_shift*0.8, box_y + 18, state.frame_time + 0.5);

            mouse_size = 1.6 - z * 0.7;
            mouse_y_pos = (box_y + 4) + z * 16;
            draw_pixel_mouse(50, mouse_y_pos, mouse_size);

            text(50, 42, 'ENTERING THE TUNNEL...', 'FontSize', 11, 'FontWeight', 'bold', 'Color', [1 0.8 0.3], 'HorizontalAlignment', 'center');

        case 'SCENE_DEADEND'
            draw_undertale_battle_box(50, 28, box_w, box_h);
            text(50, 32, '🎃 DEAD END! 🎃', 'FontSize', 24, 'FontWeight', 'bold', 'Color', [1 0.25 0.25], 'HorizontalAlignment', 'center');
            text(50, 22, 'Pip must turn back...', 'FontSize', 14, 'Color', [1 0.8 0.8], 'HorizontalAlignment', 'center');

        case 'SCENE_WEB'
            draw_undertale_battle_box(50, 28, box_w, box_h);
            draw_spider_web(50, 28, 18);
            draw_pixel_mouse(state.mouse_pos(1), state.mouse_pos(2), 1.5);
            text(50, 42, '* Spider Web Trap! (+5s)', 'FontSize', 13, 'FontWeight', 'bold', 'Color', [1 0.8 0.2], 'HorizontalAlignment', 'center');

        case 'WIN'
            state.confetti_y = state.confetti_y - state.confetti_vy;
            reset_idx = state.confetti_y < -5;
            state.confetti_y(reset_idx) = 105;
            for i = 1:length(state.confetti_x)
                plot(state.confetti_x(i), state.confetti_y(i), 's', 'MarkerFaceColor', state.confetti_colors(i,:), 'MarkerEdgeColor', 'none');
            end

            draw_undertale_battle_box(50, 48, 76, 44);
            text(50, 60, '☀️ PIP ESCAPED! ☀️', 'FontSize', 22, 'FontWeight', 'bold', 'Color', [1 0.9 0.2], 'HorizontalAlignment', 'center');
            text(50, 50, 'Pip reached the surface daylight safely!', 'FontSize', 11, 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            text(50, 44, 'Queen Muzet couldn''t catch him!', 'FontSize', 10, 'Color', [0.8 1 0.8], 'HorizontalAlignment', 'center');
            text(50, 32, '[ Press Enter to Restart ]', 'FontSize', 12, 'FontWeight', 'bold', 'Color', [0.4 1 0.5], 'HorizontalAlignment', 'center');

        case 'GAMEOVER'
            draw_undertale_battle_box(50, 50, 82, 60);

            draw_spider_web(50, 55, 20);
            draw_pixel_mouse(50, 55, 1.5);

            t_swarm = state.frame_time * 2.5;
            s1_x = 35 + sin(t_swarm * 1.8) * 8 + cos(t_swarm * 3) * 2;
            s1_y = 65 + cos(t_swarm * 2.2) * 6;

            s2_x = 65 - sin(t_swarm * 2.1) * 8 - cos(t_swarm * 2.5) * 2;
            s2_y = 65 + sin(t_swarm * 1.9) * 6;

            s3_x = 38 + cos(t_swarm * 2.5) * 7;
            s3_y = 45 - sin(t_swarm * 2.0) * 5;

            s4_x = 62 - cos(t_swarm * 2.3) * 7;
            s4_y = 45 + cos(t_swarm * 1.7) * 5;

            draw_red_eyed_spider(s1_x, s1_y);
            draw_red_eyed_spider(s2_x, s2_y);
            draw_red_eyed_spider(s3_x, s3_y);
            draw_red_eyed_spider(s4_x, s4_y);

            text(50, 32, '🕷️ PIP WAS CAUGHT! 🕷️', 'FontSize', 20, 'FontWeight', 'bold', 'Color', [1 0.2 0.2], 'HorizontalAlignment', 'center');
            text(50, 25, 'Queen Muzet''s red-eyed spiders trapped Pip!', 'FontSize', 11, 'Color', [1 1 1], 'HorizontalAlignment', 'center');
            text(50, 15, '[ Press Enter to Retry ]', 'FontSize', 12, 'FontWeight', 'bold', 'Color', [1 0.8 0.2], 'HorizontalAlignment', 'center');
    end
    drawnow;
endfunction
%Acts as the central rendering engine for the entire game. It redraws graphics, background scenery, UI boxes, text messages, and characters based on the active game mode.%

% --- Visual & Helpers ---
function draw_pixel_mouse(x, y, scale)
    rectangle('Position', [x-2.5*scale, y-2.5*scale, 5*scale, 5*scale], 'FaceColor', [0.1 0.05 0.15], 'EdgeColor', 'none');
    rectangle('Position', [x-2*scale, y-2*scale, 4*scale, 4*scale], 'FaceColor', [0.85 0.85 0.90], 'EdgeColor', 'none');
    rectangle('Position', [x-3*scale, y+1*scale, 1.6*scale, 1.6*scale], 'FaceColor', [1 0.6 0.8], 'EdgeColor', 'none');
    rectangle('Position', [x+1.4*scale, y+1*scale, 1.6*scale, 1.6*scale], 'FaceColor', [1 0.6 0.8], 'EdgeColor', 'none');
    rectangle('Position', [x-1.2*scale, y, 0.8*scale, 0.8*scale], 'FaceColor', [0 0 0], 'EdgeColor', 'none');
    rectangle('Position', [x+0.4*scale, y, 0.8*scale, 0.8*scale], 'FaceColor', [0 0 0], 'EdgeColor', 'none');
endfunction
%Draws the pixelated sprite of Pip the mouse at a specified position and scale.%

function draw_unified_aesthetic_paths(bx, by, bw, bh)
    purple_core    = [0.55 0.22 0.70];
    purple_border = [0.80 0.40 0.95];
    dark_purple    = [0.28 0.08 0.38];

    fill([bx+bw*0.40 bx+bw*0.60 bx+bw*0.28 bx+bw*0.04], [by+bh*0.12 by+bh*0.12 by+bh*0.85 by+bh*0.85], purple_border, 'EdgeColor', 'none');
    fill([bx+bw*0.40 bx+bw*0.60 bx+bw*0.60 bx+bw*0.40], [by+bh*0.12 by+bh*0.12 by+bh*0.92 by+bh*0.92], purple_border, 'EdgeColor', 'none');
    fill([bx+bw*0.40 bx+bw*0.60 bx+bw*0.96 bx+bw*0.72], [by+bh*0.12 by+bh*0.12 by+bh*0.85 by+bh*0.85], purple_border, 'EdgeColor', 'none');

    fill([bx+bw*0.42 bx+bw*0.58 bx+bw*0.26 bx+bw*0.06], [by+bh*0.12 by+bh*0.12 by+bh*0.83 by+bh*0.83], purple_core, 'EdgeColor', 'none');
    fill([bx+bw*0.42 bx+bw*0.58 bx+bw*0.58 bx+bw*0.42], [by+bh*0.12 by+bh*0.12 by+bh*0.90 by+bh*0.90], purple_core, 'EdgeColor', 'none');
    fill([bx+bw*0.42 bx+bw*0.58 bx+bw*0.94 bx+bw*0.74], [by+bh*0.12 by+bh*0.12 by+bh*0.83 by+bh*0.83], purple_core, 'EdgeColor', 'none');

    fill([bx+bw*0.38 bx+bw*0.62 bx+bw*0.62 bx+bw*0.38], [by+bh*0.02 by+bh*0.02 by+bh*0.18 by+bh*0.18], dark_purple, 'EdgeColor', purple_border, 'LineWidth', 1.5);
endfunction
%Renders the visual maze corridors and paths inside the Undertale-inspired battle box during choice screens.%

function draw_halloween_decorations(bx, by, bw, bh)
    draw_pumpkin(bx + 8, by + 4);
    draw_pumpkin(bx + bw - 8, by + 4);
    plot([bx, bx+5], [by+bh, by+bh-5], 'Color', [0.7 0.6 0.8]);
    plot([bx+bw, bx+bw-5], [by+bh, by+bh-5], 'Color', [0.7 0.6 0.8]);
endfunction
%Adds thematic Halloween elements like pumpkins and hanging decorations around the game boundaries.%

function draw_pumpkin(x, y)
    rectangle('Position', [x-1.5, y-1.5, 3, 3], 'Curvature', [0.5 0.5], 'FaceColor', [1 0.5 0], 'EdgeColor', 'none');
    rectangle('Position', [x-0.3, y+1.2, 0.6, 1], 'FaceColor', [0.2 0.6 0.1], 'EdgeColor', 'none');
endfunction
%Helper function that draws an individual pixelated pumpkin decoration.%

function draw_subtle_torch(x, y, frame)
    flicker = sin(frame * 6) * 0.4;
    for r = [6, 3]
        alpha_val = (8 - r) / 8;
        fill(x + cos(0:0.5:2*pi)*(r+flicker), y + 2 + sin(0:0.5:2*pi)*(r+flicker), ...
             [0.8, 0.5, 0.1] * alpha_val + [0.03, 0.01, 0.05]*(1-alpha_val), 'EdgeColor', 'none');
    end
    rectangle('Position', [x-0.8, y-5, 1.6, 5], 'FaceColor', [0.3 0.15 0.05], 'EdgeColor', 'none');
    fill([x-1.5, x, x+1.5], [y, y+3+flicker, y], [1 0.7 0.1], 'EdgeColor', 'none');
endfunction
%Renders an animated, flickering torch with a glowing aura to enhance the atmospheric dungeon lighting.%

function draw_arrow_button(x, y, txt)
    rectangle('Position', [x-10 y-2.5 20 5], 'Curvature', [0.3 0.3], 'FaceColor', [0.12 0.06 0.18], 'EdgeColor', [1 0.6 0.2], 'LineWidth', 1.5);
    text(x, y, txt, 'FontSize', 10, 'FontWeight', 'bold', 'Color', [1 0.9 0.6], 'HorizontalAlignment', 'center');
endfunction
%Draws interactive directional buttons (< LEFT, ^ UP ^, RIGHT >) for the player to click during choice phases.%

function draw_red_eyed_spider(x, y)
    fill(x+cos(0:0.5:2*pi)*3, y+sin(0:0.5:2*pi)*3, [0.08 0.08 0.08], 'EdgeColor', 'none');
    for leg = [-1, 1]
        plot([x, x+leg*5, x+leg*7], [y, y+3, y-3], 'Color', [0.15 0.15 0.15], 'LineWidth', 2);
        plot([x, x+leg*5, x+leg*7], [y, y, y-5], 'Color', [0.15 0.15 0.15], 'LineWidth', 2);
    end
    plot(x-1, y+1, 'ro', 'MarkerSize', 4, 'MarkerFaceColor', [1 0 0]);
    plot(x+1, y+1, 'ro', 'MarkerSize', 4, 'MarkerFaceColor', [1 0 0]);
endfunction
%Draws a menacing spider with glowing red eyes used in the Game Over swarm sequence.%

function draw_pixelated_castle_scenery()
    purple_dark  = [0.18, 0.06, 0.25];
    purple_mid   = [0.35, 0.15, 0.45];

    t = 0:0.2:pi;
    for i = 1:length(t)
        px = 50 + cos(t(i))*14; py = 72 + sin(t(i))*14;
        rectangle('Position', [floor(px) floor(py) 2 2], 'FaceColor', purple_dark, 'EdgeColor', 'none');
    end
    rectangle('Position', [45 84 10 6], 'FaceColor', purple_dark, 'EdgeColor', 'none');

    rectangle('Position', [4 50 20 40], 'FaceColor', purple_mid, 'EdgeColor', [0.1 0.0 0.15], 'LineWidth', 2);
    rectangle('Position', [24 55 12 30], 'FaceColor', purple_dark, 'EdgeColor', 'none');

    rectangle('Position', [76 50 20 40], 'FaceColor', purple_mid, 'EdgeColor', [0.1 0.0 0.15], 'LineWidth', 2);
    rectangle('Position', [64 55 12 30], 'FaceColor', purple_dark, 'EdgeColor', 'none');
endfunction
%Renders pixel art castle towers and background castle silhouettes for the underground layout.%

function draw_surface_scenery()
    rectangle('Position', [0 0 100 30], 'FaceColor', [0.2 0.6 0.25], 'EdgeColor', 'none');
    fill([0 30 60 0], [30 50 30 30], [0.18 0.52 0.22], 'EdgeColor', 'none');
    fill([40 70 100 40], [30 52 30 30], [0.18 0.52 0.22], 'EdgeColor', 'none');
    fill(15 + cos(0:0.2:2*pi)*8, 80 + sin(0:0.2:2*pi)*8, [1 0.9 0.2], 'EdgeColor', 'none');
endfunction
%Draws the bright outdoor surface scenery and sun when the player successfully wins the game.%

function draw_undertale_battle_box(cx, cy, w, h) %serves as a universal visual aid for multiple panels
    x = cx - w/2; y = cy - h/2;
    rectangle('Position', [x y w h], 'FaceColor', [0.04 0.01 0.06], 'EdgeColor', [1 0.6 0.2], 'LineWidth', 3.5);
endfunction

function draw_pixel_cloud(x, y, scale) % his function is used to draw a pixel art-style cloud at position (x, y) with an adjustable size (scale).
    c_col = [0.85 0.80 0.92];
    rectangle('Position', [x y 12*scale 4*scale], 'FaceColor', c_col, 'EdgeColor', 'none');
    rectangle('Position', [x+2*scale y+3*scale 8*scale 3*scale], 'FaceColor', c_col, 'EdgeColor', 'none');
endfunction

function draw_spider_web(x, y, radius) %Draw a spider’s web centred at the point (x, y) with a given radius.
    t = 0:pi/4:2*pi; web_color = [0.75 0.65 0.85];
    for r = [0.4, 0.8] * radius
        plot(x + cos(t)*r, y + sin(t)*r, 'Color', web_color, 'LineWidth', 1);
    end
    for i = 1:length(t)
        plot([x, x + cos(t(i))*radius], [y, y + sin(t(i))*radius], 'Color', web_color, 'LineWidth', 1);
    end
endfunction
