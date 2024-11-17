local Players = game:GetService("Players")
local ScreenGui = Instance.new("ScreenGui")
local MainFrame, Title, KeyFrame = Instance.new("Frame"), Instance.new("TextLabel"), Instance.new("Frame")
local KeyTitle, KeyInput, SubmitButton = Instance.new("TextLabel"), Instance.new("TextBox"), Instance.new("TextButton")
local GetKeyButton = Instance.new("TextButton")  -- Кнопка для получения ключа
local Notification = Instance.new("TextLabel")  -- Уведомление о копировании
local TabsFrame, ContentFrame = Instance.new("Frame"), Instance.new("Frame")
local ESPFrame, PlayerFrame, MiscFrame, InfoFrame = Instance.new("Frame"), Instance.new("Frame"), Instance.new("Frame"), Instance.new("Frame")
local dragging, dragStart, startPos = false, nil, nil
local godModeEnabled = false

-- Настройки GUI
ScreenGui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")
MainFrame.Size = UDim2.new(0, 500, 0, 400)
MainFrame.Position = UDim2.new(0.5, -245, 0.5, -200)
MainFrame.BackgroundColor3, MainFrame.BorderSizePixel = Color3.fromRGB(40, 40, 40), 0
MainFrame.BackgroundTransparency = 1
MainFrame.Parent = ScreenGui

-- Заголовок
Title.Size = UDim2.new(1, 0, 0, 50)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundColor3, Title.Text = Color3.fromRGB(100, 100, 100), "Skull Hub"
Title.TextColor3, Title.Font, Title.FontSize, Title.TextScaled = Color3.fromRGB(255, 255, 255), Enum.Font.GothamBold, Enum.FontSize.Size24, true
Title.Parent = MainFrame

-- Перетаскивание
Title.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

Title.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
		MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + (input.Position.X - dragStart.X), startPos.Y.Scale, startPos.Y.Offset + (input.Position.Y - dragStart.Y))
	end
end)

-- Фрейм для ключа
KeyFrame.Size = UDim2.new(0, 300, 0, 200)
KeyFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
KeyFrame.BackgroundColor3, KeyFrame.BorderSizePixel = Color3.fromRGB(40, 40, 40), 0
KeyFrame.Parent = ScreenGui

-- Заголовок для ключа
KeyTitle.Size = UDim2.new(1, 0, 0, 50)
KeyTitle.Position = UDim2.new(0, 0, 0, 0)
KeyTitle.BackgroundColor3, KeyTitle.Text = Color3.fromRGB(100, 100, 100), "ENTER KEY"
KeyTitle.TextColor3, KeyTitle.Font, KeyTitle.FontSize, KeyTitle.TextScaled = Color3.fromRGB(255, 255, 255), Enum.Font.GothamBold, Enum.FontSize.Size24, true
KeyTitle.Parent = KeyFrame

-- Поле ввода ключа
KeyInput.Size = UDim2.new(1, -20, 0, 40)
KeyInput.Position = UDim2.new(0, 10, 0, 60)
KeyInput.BackgroundColor3, KeyInput.TextColor3, KeyInput.Font, KeyInput.PlaceholderText = Color3.fromRGB(70, 70, 70), Color3.fromRGB(255, 255, 255), Enum.Font.Gotham, "Enter your key here"
KeyInput.Parent = KeyFrame

-- Кнопка отправки
SubmitButton.Size = UDim2.new(0.5, 0, 0, 40)
SubmitButton.Position = UDim2.new(0.25, 0, 0, 110)
SubmitButton.BackgroundColor3, SubmitButton.Text = Color3.fromRGB(90, 90, 90), "Submit"
SubmitButton.TextColor3, SubmitButton.Font, SubmitButton.FontSize, SubmitButton.TextScaled = Color3.fromRGB(255, 255, 255), Enum.Font.GothamBold, Enum.FontSize.Size18, true
SubmitButton.Parent = KeyFrame

-- Кнопка "Get Key"
GetKeyButton.Size = UDim2.new(1, 0, 0, 40)
GetKeyButton.Position = UDim2.new(0, 0, 0, 160)  -- Позиция ниже поля ввода
GetKeyButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
GetKeyButton.Text = "Get Key"
GetKeyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
GetKeyButton.Font = Enum.Font.GothamBold
GetKeyButton.FontSize = Enum.FontSize.Size18
GetKeyButton.TextScaled = true
GetKeyButton.Parent = KeyFrame

-- Уведомление о копировании
Notification.Size = UDim2.new(1, 0, 0, 50)
Notification.Position = UDim2.new(0, 0, 0, 210)
Notification.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Notification.TextColor3 = Color3.fromRGB(255, 255, 255)
Notification.Font = Enum.Font.GothamBold
Notification.FontSize = Enum.FontSize.Size24
Notification.TextScaled = true
Notification.Text = ""
Notification.Parent = KeyFrame

-- Создание вкладок
local function createTabButton(name, position)
	local button = Instance.new("TextButton")
	button.Size = UDim2.new(1, 0, 0, 40)
	button.Position = position
	button.BackgroundColor3, button.TextColor3, button.Font, button.FontSize, button.TextScaled = Color3.fromRGB(90, 90, 90), Color3.fromRGB(255, 255, 255), Enum.Font.GothamBold, Enum.FontSize.Size18, true
	button.Text = name
	button.Parent = TabsFrame
	return button
end

-- Создание фреймов и кнопок
TabsFrame.Size = UDim2.new(0, 120, 1, -50)
TabsFrame.Position = UDim2.new(0, 0, 0, 50)
TabsFrame.BackgroundColor3, TabsFrame.Parent = Color3.fromRGB(70, 70, 70), MainFrame

local ESPButton = createTabButton("ESP", UDim2.new(0, 0, 0, 0))
local PlayerButton = createTabButton("Player", UDim2.new(0, 0, 0, 50))
local MiscButton = createTabButton("Misc", UDim2.new(0, 0, 0, 100))
local InfoButton = createTabButton("Info", UDim2.new(0, 0, 0, 150))

ContentFrame.Size = UDim2.new(1, -120, 1, -50)
ContentFrame.Position = UDim2.new(0, 120, 0, 50)
ContentFrame.BackgroundColor3, ContentFrame.Parent = Color3.fromRGB(50, 50, 50), MainFrame

local function createContentFrame()
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 1, 0)
	frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	frame.Visible = false
	frame.Parent = ContentFrame
	return frame
end

ESPFrame, PlayerFrame, MiscFrame, InfoFrame = createContentFrame(), createContentFrame(), createContentFrame(), createContentFrame()

-- Показать фрейм
local function showFrame(frameToShow)
	for _, frame in pairs(ContentFrame:GetChildren()) do
		if frame:IsA("Frame") then frame.Visible = false end
	end
	frameToShow.Visible = true
	frameToShow.BackgroundTransparency = 1
	frameToShow:TweenSize(UDim2.new(1, 0, 1, 0), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.5, true)
	frameToShow:TweenPosition(UDim2.new(0, 120, 0, 50), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.5, true)
	frameToShow.BackgroundTransparency = 0
end

-- Кнопка ESP Players
local espPlayersButton = Instance.new("TextButton")
espPlayersButton.Size = UDim2.new(0.5, 0, 0, 40)
espPlayersButton.Position = UDim2.new(0.25, 0, 0, 20)
espPlayersButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
espPlayersButton.Text = "ESP Players"
espPlayersButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espPlayersButton.Font = Enum.Font.GothamBold
espPlayersButton.FontSize = Enum.FontSize.Size18
espPlayersButton.TextScaled = true
espPlayersButton.Parent = ESPFrame

-- Кнопка Godmode
local godmodeButton = Instance.new("TextButton")
godmodeButton.Size = UDim2.new(0.5, 0, 0, 40)
godmodeButton.Position = UDim2.new(0.25, 0, 0, 70)
godmodeButton.BackgroundColor3 = Color3.fromRGB(90, 90, 90)
godmodeButton.Text = "Godmode"
godmodeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
godmodeButton.Font = Enum.Font.GothamBold
godmodeButton.FontSize = Enum.FontSize.Size18
godmodeButton.TextScaled = true
godmodeButton.Parent = PlayerFrame

-- Активация ESP с именами и обводкой
local function activateESP()
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= Players.LocalPlayer then
			local highlight = Instance.new("Highlight")
			highlight.Parent = player.Character or player.CharacterAdded:Wait()
			highlight.Adornee = player.Character:WaitForChild("HumanoidRootPart")
			highlight.FillColor, highlight.FillTransparency = Color3.new(1, 0, 0), 0.5  -- Яркая красная обводка
			highlight.OutlineColor, highlight.OutlineTransparency = Color3.new(1, 0, 0), 0  -- Яркая красная обводка

			-- Создание BillboardGui для отображения имени
			local billboard = Instance.new("BillboardGui")
			billboard.Adornee = player.Character:WaitForChild("Head")
			billboard.Size = UDim2.new(0, 200, 0, 50) -- Увеличиваем размер
			billboard.StudsOffset = Vector3.new(0, 2, 0) -- Смещение над головой
			billboard.AlwaysOnTop = true -- Имя всегда сверху
			billboard.Parent = player.Character.Head

			local nameLabel = Instance.new("TextLabel")
			nameLabel.Size = UDim2.new(1, 0, 1, 0)
			nameLabel.BackgroundTransparency = 1
			nameLabel.TextColor3 = Color3.new(1, 0, 0) -- Красный цвет текста
			nameLabel.Text = player.Name
			nameLabel.TextScaled = true -- Увеличиваем текст
			nameLabel.Parent = billboard
		end
	end
end

-- Функция активации Godmode
local function toggleGodMode()
	godModeEnabled = not godModeEnabled
	local player = Players.LocalPlayer
	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:WaitForChild("Humanoid")

	if godModeEnabled then
		humanoid.MaxHealth = math.huge -- Устанавливаем максимальное здоровье
		humanoid.Health = humanoid.MaxHealth -- Устанавливаем текущее здоровье равным максимальному
		humanoid.HealthChanged:Connect(function(health)
			if health < humanoid.MaxHealth then
				humanoid.Health = humanoid.MaxHealth -- Восстанавливаем здоровье
			end
		end)
	else
		humanoid.MaxHealth = 100 -- Возвращаем максимальное здоровье к норме
	end
end

-- Обработчики кликов
SubmitButton.MouseButton1Click:Connect(function()
	if KeyInput.Text == "JoinMyDiscord" then
		KeyFrame.Visible = false
		MainFrame.Visible = true
		MainFrame:TweenBackgroundTransparency(0, Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.5, true)
	else
		KeyInput.Text = ""
		KeyInput.PlaceholderText = "Invalid key, try again"
	end
end)

GetKeyButton.MouseButton1Click:Connect(function()
	setclipboard("https://discord.gg/Ma688VtARC")  -- Копируем ссылку в буфер обмена
	Notification.Text = "Copied to clipboard!"  -- Уведомление
	Notification.Visible = true  -- Показываем уведомление
	wait(2)  -- Ждем 2 секунды
	Notification.Visible = false  -- Скрываем уведомление
end)

ESPButton.MouseButton1Click:Connect(function() showFrame(ESPFrame) end)
PlayerButton.MouseButton1Click:Connect(function() showFrame(PlayerFrame) end)
MiscButton.MouseButton1Click:Connect(function() showFrame(MiscFrame) end)
InfoButton.MouseButton1Click:Connect(function() showFrame(InfoFrame) end)
espPlayersButton.MouseButton1Click:Connect(activateESP)
godmodeButton.MouseButton1Click:Connect(toggleGodMode)

-- Изначально показываем окно для ввода ключа
MainFrame.Visible = false
KeyFrame.Visible = true
