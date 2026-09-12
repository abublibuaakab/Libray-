local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

local Library = {}

local Theme = {
    Background = Color3.fromRGB(14, 14, 17),
    Secondary = Color3.fromRGB(19, 19, 23),
    Element = Color3.fromRGB(24, 24, 29),
    Text = Color3.fromRGB(245, 245, 248),
    SubText = Color3.fromRGB(150, 150, 158),
    Accent = Color3.fromRGB(120, 100, 255),
    Stroke = Color3.fromRGB(70, 70, 80)
}

local Fast = TweenInfo.new(
    0.15,
    Enum.EasingStyle.Quad,
    Enum.EasingDirection.Out
)

local Smooth = TweenInfo.new(
    0.25,
    Enum.EasingStyle.Quart,
    Enum.EasingDirection.Out
)

local function Create(className, properties)
    local object = Instance.new(className)

    for property, value in pairs(properties or {}) do
        object[property] = value
    end

    return object
end

local function Corner(parent, radius)
    return Create("UICorner", {
        CornerRadius = UDim.new(0, radius or 8),
        Parent = parent
    })
end

local function Stroke(parent, color, thickness)
    return Create("UIStroke", {
        Color = color or Theme.Stroke,
        Thickness = thickness or 1,
        Transparency = 0,
        Parent = parent
    })
end

local function Tween(object, properties, info)
    TweenService:Create(
        object,
        info or Fast,
        properties
    ):Play()
end

local function MakeDraggable(handle, object)
    local dragging = false
    local dragStart
    local startPosition

    handle.InputBegan:Connect(function(input)
        if input.UserInputType ~= Enum.UserInputType.MouseButton1
            and input.UserInputType ~= Enum.UserInputType.Touch then
            return
        end

        dragging = true
        dragStart = input.Position
        startPosition = object.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end)

    UserInputService.InputChanged:Connect(function(input)
        if not dragging then
            return
        end

        if input.UserInputType ~= Enum.UserInputType.MouseMovement
            and input.UserInputType ~= Enum.UserInputType.Touch then
            return
        end

        local delta = input.Position - dragStart

        object.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end)
end

Library.Icons = {
    Home = "",
    User = "",
    Settings = "",
    Search = "",
    Notifications = "",
    Mail = "",
    Folder = "",
    Apps = "",
    Favorite = "",
    Star = "",
    Add = "",
    Remove = "",
    Check = "",
    Close = "",
    Eye = "",
    EyeOff = "",
    Back = "",
    Forward = "",
    Up = "",
    Down = "",
    Refresh = "",
    Link = "",
    Delete = "",
    Download = "",
    Upload = "",
    Location = "",
    Map = "",
    Camera = "",
    Image = "",
    Play = "",
    Pause = "",
    Stop = "",
    Volume = "",
    Mute = "",
    Light = "",
    Dark = "",
    Battery = "",
    Wifi = "",
    Signal = "",
    External = ""
}

function Library:CreateWindow(config)
    config = config or {}

    local Title = config.Title or "Minimal UI"
    local Subtitle = config.Subtitle or "Library"

    local ScreenGui = Create("ScreenGui", {
        Name = "MinimalUI",
        ResetOnSpawn = false,
        ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
        Parent = LocalPlayer:WaitForChild("PlayerGui")
    })

    local Window = Create("Frame", {
        Name = "Window",
        Size = UDim2.fromOffset(500, 340),
        Position = UDim2.new(0.5, -250, 0.5, -170),
        BackgroundColor3 = Theme.Background,
        BackgroundTransparency = 0.25,
        BorderSizePixel = 0,
        Parent = ScreenGui
    })

    Corner(Window, 10)
    Stroke(Window)

    local Header = Create("Frame", {
        Name = "Header",
        Size = UDim2.new(1, 0, 0, 55),
        BackgroundTransparency = 1,
        Parent = Window
    })

    local TitleLabel = Create("TextLabel", {
        Name = "Title",
        Position = UDim2.fromOffset(18, 8),
        Size = UDim2.new(1, -110, 0, 22),
        BackgroundTransparency = 1,
        Text = Title,
        TextColor3 = Theme.Text,
        TextSize = 15,
        Font = Enum.Font.GothamSemibold,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = Header
    })

    local SubtitleLabel = Create("TextLabel", {
        Name = "Subtitle",
        Position = UDim2.fromOffset(18, 29),
        Size = UDim2.new(1, -110, 0, 17),
        BackgroundTransparency = 1,
        Text = Subtitle,
        TextColor3 = Theme.SubText,
        TextSize = 11,
        Font = Enum.Font.Gotham,
        TextXAlignment = Enum.TextXAlignment.Left,
        Parent = Header
    })

    local Minimize = Create("TextButton", {
        Name = "Minimize",
        Position = UDim2.new(1, -65, 0, 17),
        Size = UDim2.fromOffset(20, 20),
        BackgroundTransparency = 1,
        Text = "−",
        TextColor3 = Theme.SubText,
        TextSize = 18,
        Font = Enum.Font.GothamMedium,
        AutoButtonColor = false,
        Parent = Header
    })

    local Close = Create("TextButton", {
        Name = "Close",
        Position = UDim2.new(1, -35, 0, 17),
        Size = UDim2.fromOffset(20, 20),
        BackgroundTransparency = 1,
        Text = "×",
        TextColor3 = Theme.SubText,
        TextSize = 18,
        Font = Enum.Font.GothamMedium,
        AutoButtonColor = false,
        Parent = Header
    })

    Minimize.MouseEnter:Connect(function()
        Tween(Minimize, {
            TextColor3 = Theme.Text
        })
    end)

    Minimize.MouseLeave:Connect(function()
        Tween(Minimize, {
            TextColor3 = Theme.SubText
        })
    end)

    Close.MouseEnter:Connect(function()
        Tween(Close, {
            TextColor3 = Color3.fromRGB(255, 100, 100)
        })
    end)

    Close.MouseLeave:Connect(function()
        Tween(Close, {
            TextColor3 = Theme.SubText
        })
    end)

    MakeDraggable(Header, Window)

    local Content = Create("Frame", {
        Name = "Content",
        Position = UDim2.fromOffset(12, 55),
        Size = UDim2.new(1, -24, 1, -67),
        BackgroundTransparency = 1,
        Parent = Window
    })

    local TabBar = Create("Frame", {
        Name = "Tabs",
        Position = UDim2.fromOffset(0, 0),
        Size = UDim2.new(0, 115, 1, 0),
        BackgroundColor3 = Theme.Secondary,
        BackgroundTransparency = 0.25,
        BorderSizePixel = 0,
        Parent = Content
    })

    Corner(TabBar, 8)

    Create("UIListLayout", {
        Padding = UDim.new(0, 5),
        SortOrder = Enum.SortOrder.LayoutOrder,
        Parent = TabBar
    })

    local Pages = Create("Frame", {
        Name = "Pages",
        Position = UDim2.fromOffset(125, 0),
        Size = UDim2.new(1, -125, 1, 0),
        BackgroundTransparency = 1,
        Parent = Content
    })

    local Tabs = {}
    local Minimized = false

    Minimize.MouseButton1Click:Connect(function()
        Minimized = not Minimized

        if Minimized then
            Content.Visible = false

            Tween(Window, {
                Size = UDim2.fromOffset(500, 55)
            }, Smooth)

            Tween(Minimize, {
                Rotation = 180
            })
        else
            Content.Visible = true

            Tween(Window, {
                Size = UDim2.fromOffset(500, 340)
            }, Smooth)

            Tween(Minimize, {
                Rotation = 0
            })
        end
    end)

    Close.MouseButton1Click:Connect(function()
        Tween(Window, {
            Size = UDim2.fromOffset(500, 0)
        }, Smooth)

        task.wait(0.25)

        ScreenGui:Destroy()
    end)

    local WindowObject = {}

    function WindowObject:CreateTab(name)
        local Page = Create("ScrollingFrame", {
            Name = name,
            Size = UDim2.fromScale(1, 1),
            BackgroundTransparency = 1,
            BorderSizePixel = 0,
            ScrollBarThickness = 2,
            ScrollBarImageColor3 = Theme.Accent,
            CanvasSize = UDim2.new(0, 0, 0, 0),
            AutomaticCanvasSize = Enum.AutomaticSize.Y,
            Visible = false,
            Parent = Pages
        })

        Create("UIListLayout", {
            Padding = UDim.new(0, 7),
            SortOrder = Enum.SortOrder.LayoutOrder,
            Parent = Page
        })

        Create("UIPadding", {
            PaddingTop = UDim.new(0, 2),
            PaddingBottom = UDim.new(0, 5),
            PaddingLeft = UDim.new(0, 2),
            PaddingRight = UDim.new(0, 5),
            Parent = Page
        })

        local TabButton = Create("TextButton", {
            Size = UDim2.new(1, -10, 0, 35),
            BackgroundColor3 = Theme.Element,
            BackgroundTransparency = 1,
            BorderSizePixel = 0,
            Text = name,
            TextColor3 = Theme.SubText,
            TextSize = 12,
            Font = Enum.Font.GothamMedium,
            AutoButtonColor = false,
            Parent = TabBar
        })

        Corner(TabButton, 7)

        TabButton.MouseEnter:Connect(function()
            if not Page.Visible then
                Tween(TabButton, {
                    BackgroundTransparency = 0.4,
                    TextColor3 = Theme.Text
                })
            end
        end)

        TabButton.MouseLeave:Connect(function()
            if not Page.Visible then
                Tween(TabButton, {
                    BackgroundTransparency = 1,
                    TextColor3 = Theme.SubText
                })
            end
        end)

        TabButton.MouseButton1Click:Connect(function()
            for _, page in ipairs(Pages:GetChildren()) do
                if page:IsA("ScrollingFrame") then
                    page.Visible = false
                end
            end

            for _, button in ipairs(TabBar:GetChildren()) do
                if button:IsA("TextButton") then
                    Tween(button, {
                        BackgroundTransparency = 1,
                        TextColor3 = Theme.SubText
                    })
                end
            end

            Page.Visible = true

            Tween(TabButton, {
                BackgroundTransparency = 0,
                TextColor3 = Theme.Text
            })
        end)

        local Tab = {}

        function Tab:CreateLabel(text)
            return Create("TextLabel", {
                Size = UDim2.new(1, 0, 0, 28),
                BackgroundTransparency = 1,
                Text = text,
                TextColor3 = Theme.SubText,
                TextSize = 11,
                Font = Enum.Font.Gotham,
                TextXAlignment = Enum.TextXAlignment.Left,
                Parent = Page
            })
        end

        function Tab:CreateButton(options)
            options = options or {}

            local Button = Create("TextButton", {
                Size = UDim2.new(1, 0, 0, 40),
                BackgroundColor3 = Theme.Element,
                BorderSizePixel = 0,
                Text = options.Name or "Button",
                TextColor3 = Theme.Text,
                TextSize = 12,
                Font = Enum.Font.GothamMedium,
                AutoButtonColor = false,
                Parent = Page
            })

            Corner(Button, 7)
            Stroke(Button)

            Button.MouseEnter:Connect(function()
                Tween(Button, {
                    BackgroundColor3 = Color3.fromRGB(30, 30, 36)
                })
            end)

            Button.MouseLeave:Connect(function()
                Tween(Button, {
                    BackgroundColor3 = Theme.Element
                })
            end)

            Button.MouseButton1Click:Connect(function()
                if options.Callback then
                    task.spawn(options.Callback)
                end
            end)

            return Button
        end

        function Tab:CreateToggle(options)
            options = options or {}

            local Enabled = options.Default or false

            local Holder = Create("Frame", {
                Size = UDim2.new(1, 0, 0, 42),
                BackgroundColor3 = Theme.Element,
                BorderSizePixel = 0,
                Parent = Page
            })

            Corner(Holder, 7)
            Stroke(Holder)

            Create("TextLabel", {
                Position = UDim2.fromOffset(12, 0),
                Size = UDim2.new(1, -70, 1, 0),
                BackgroundTransparency = 1,
                Text = options.Name or "Toggle",
                TextColor3 = Theme.Text,
                TextSize = 12,
                Font = Enum.Font.GothamMedium,
                TextXAlignment = Enum.TextXAlignment.Left,
                Parent = Holder
            })

            local Switch = Create("TextButton", {
                Position = UDim2.new(1, -52, 0.5, -10),
                Size = UDim2.fromOffset(40, 20),
                BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                BorderSizePixel = 0,
                Text = "",
                AutoButtonColor = false,
                Parent = Holder
            })

            Corner(Switch, 10)

            local SwitchGradient = Create("UIGradient", {
                Color = ColorSequence.new({
                    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 85, 85)),
                    ColorSequenceKeypoint.new(1, Color3.fromRGB(170, 0, 0))
                }),
                Parent = Switch
            })

            local Circle = Create("Frame", {
                Position = UDim2.fromOffset(3, 3),
                Size = UDim2.fromOffset(14, 14),
                BackgroundColor3 = Color3.fromRGB(220, 220, 225),
                BorderSizePixel = 0,
                Parent = Switch
            })

            Corner(Circle, 10)

            local function Update()
                if Enabled then
                    SwitchGradient.Color = ColorSequence.new({
                        ColorSequenceKeypoint.new(0, Color3.fromRGB(85, 255, 127)),
                        ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 170, 0))
                    })
                    Tween(Circle, {
                        Position = UDim2.new(1, -17, 0, 3)
                    })
                else
                    SwitchGradient.Color = ColorSequence.new({
                        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 85, 85)),
                        ColorSequenceKeypoint.new(1, Color3.fromRGB(170, 0, 0))
                    })
                    Tween(Circle, {
                        Position = UDim2.fromOffset(3, 3)
                    })
                end

                if options.Callback then
                    task.spawn(options.Callback, Enabled)
                end
            end

            Switch.MouseButton1Click:Connect(function()
                Enabled = not Enabled
                Update()
            end)

            Update()

            return {
                Set = function(_, value)
                    Enabled = value
                    Update()
                end,

                Get = function()
                    return Enabled
                end
            }
        end

        function Tab:CreateSpacer(height)
            return Create("Frame", {
                Size = UDim2.new(1, 0, 0, height or 5),
                BackgroundTransparency = 1,
                Parent = Page
            })
        end

        if #Tabs == 0 then
            Page.Visible = true

            Tween(TabButton, {
                BackgroundTransparency = 0,
                TextColor3 = Theme.Text
            })
        end

        table.insert(Tabs, Tab)

        return Tab
    end

    return WindowObject
end

return Library
