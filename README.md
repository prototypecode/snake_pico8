# snake_pico8

```lua

scale = 6
speedinticks = 2
tick = 1
addlength = 2

game = {}
snake = {}
food = {}

function newgame()
    game._draw = drawgame
    game._update = updategame
    snake.x = 1
    snake.y = 1
    snake.deltay = 0
    snake.deltax = 0
    snake.scale = scale
    snake.count = 1
    snake.body = {}
    snake.adding = false
    snake.addcount = 0
    food.scale = scale
    food:move()
end

function drawmenu()
    cls(1)
    print("press z", 50, 50)
end

function updatemenu()
    if btn(4) then
        newgame()
    end
end

function drawrip()
    cls(1)
    print("press z to play again.", 24, 50, 7)
end

function updaterip()
    if btn(4) then newgame() end
end

function updategame()
    snake:keypress()
    if tick == speedinticks then
        snake:move()
        tick = 1
    else
        tick += 1
    end
end

function drawgame()
    cls(0)
    color(1)
    rect(0, 0, 126, 126)
    food:draw()
    snake:draw()
end

function _init()
    game._update = updatemenu
    game._draw = drawmenu
end

function _update()
    game._update()
end

function _draw()
    game._draw()
end

function food:draw()
    color(10)
    rectfill(self.x, self.y, self.x + self.scale-2, self.y + self.scale-2)
end

function food:move()
    self.x = (flr(rnd(120 / self.scale)) * self.scale) + 1
    self.y = (flr(rnd(120 / self.scale)) * self.scale) + 1
end

function snake:draw()
    color(7)
   
    rectfill(self.x, self.y,self.x + self.scale-2, self.y + self.scale-2)
   
    for i=1, #self.body do
        color(11)
        rectfill(self.body[i].x, self.body[i].y, self.body[i].x + self.scale-2, self.body[i].y + self.scale-2)
    end
end

function snake:keypress()
    if btn(0) and
       self.deltax!=1 then -- left
        self.deltax=-1
        self.deltay=0
    end
    if btn(1) and
       self.deltax!=-1 then -- right
        self.deltax=1
        self.deltay=0
    end
    if btn(2) and
       self.deltay!=1 then -- up
        self.deltay=-1
        self.deltax=0
    end
    if btn(3) and
       self.deltay!=-1 then -- down
        self.deltay=1
        self.deltax=0
    end
end

function snake:move()
    self:eat() 
   
    self.x+=self.deltax*(self.scale)
    self.y+=self.deltay*(self.scale)
   
    for i=1,#self.body do
        if self.body[i].x==self.x and self.body[i].y==self.y then
            game._draw=drawrip
            game._update=updaterip
        end
    end

    -- if your off the sceen die
    if self.x>126 then 
        game._draw=drawrip 
        game._update=updaterip end
    if self.x<1 then 
        game._draw=drawrip 
        game._update=updaterip end
    if self.y>126 then 
        game._draw=drawrip 
        game._update=updaterip end
    if self.y<1 then 
        game._draw=drawrip 
        game._update=updaterip end
end

function snake:eat()
    if self.x==food.x and self.y==food.y then
        food:move()
        self.adding=true
        sfx(0)
    end

    if self.adding then
        if self.addcount==addlength then
            self.adding=false
            self.addcount=0
        else
            self.addcount+=1
            self.count+=1
        end
    end
    if self.count-1==#self.body then
        for i=1,#self.body-1 do 
            self.body[i] = self.body[i+1]
        end
    end
   
    self.body[self.count-1] = {}
    self.body[self.count-1].x = self.x
    self.body[self.count-1].y = self.y
end


```
