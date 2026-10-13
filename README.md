--==================================================--
--            BlockSpin Script Hub                  --
--               Created by: dev night              --
--==================================================--

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "BlockSpin Hub 🔪 | dev night",
   LoadingTitle = "جارٍ تحميل سكربت BlockSpin...",
   LoadingSubtitle = "by dev night",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DevNightConfigs",
      FileName = "BlockSpinHub"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink",
      RememberJoins = true
   },
   KeySystem = false
})

-- المتغيرات الأساسية
local Settings = {
   AutoFarmCash = false,
   AutoCollect = false,
   WalkSpeed = 16,
   JumpPower = 50,
   HitboxSize = 2,
   AutoAttack = false
}

-- 1. تبويب الرئيسية (Home Tab)
local HomeTab = Window:CreateTab("الرئيسية", 4483362458)
HomeTab:CreateSection("معلومات الهب")

HomeTab:CreateParagraph({
   Title = "مرحباً بك في BlockSpin Hub",
   Content = "هذا الهب مخصص لتطوير أداؤك، جمع الفلوس، والسيطرة على الماب بسهولة.\nالحقوق محفوظة لـ: dev night"
})

HomeTab:CreateButton({
   Name = "نسخ حقوق الصانع (dev night)",
   Callback = function()
      setclipboard("dev night")
      Rayfield:Notify({
         Title = "تم النسخ",
         Content = "تم نسخ الحقوق بنجاح إلى الحافظة!",
         Duration = 3,
      })
   end,
})

-- 2. تبويب الفارم والفلوس (Auto Farm Tab)
local FarmTab = Window:CreateTab("الفارم والفلوس", 4483362458)
FarmTab:CreateSection("خيارات جمع الأرباح")

FarmTab:CreateToggle({
   Name = "تفعيل Auto Farm Cash (الفلوس/الـ ATMs)",
   CurrentValue = false,
   Flag = "FarmCashFlag",
   Callback = function(Value)
      Settings.AutoFarmCash = Value
   end,
})

FarmTab:CreateToggle({
   Name = "تجميع Drops والأشياء بالأرض تلقائياً",
   CurrentValue = false,
   Flag = "AutoCollectFlag",
   Callback = function(Value)
      Settings.AutoCollect = Value
   end,
})

-- 3. تبويب القتال والأسلحة (Combat Tab)
local CombatTab = Window:CreateTab("القتال والأسلحة", 4483362458)
CombatTab:CreateSection("أدوات التفوق بالقتال")

CombatTab:CreateToggle({
   Name = "تفعيل Auto Attack / ضرب تلقائي",
   CurrentValue = false,
   Flag = "AutoAttackFlag",
   Callback = function(Value)
      Settings.AutoAttack = Value
   end,
})

CombatTab:CreateSlider({
   Name = "توسيع حجمHitbox اللاعبين",
   Range = {2, 20},
   Increment = 1,
   Suffix = "Studs",
   CurrentValue = 2,
   Flag = "HitboxFlag",
   Callback = function(Value)
      Settings.HitboxSize = Value
   end,
})

-- 4. تبويب الانتقال السريع (Teleports Tab)
local TP_Tab = Window:CreateTab("الانتقال السريع", 4483362458)
TP_Tab:CreateSection("أماكن هامة في الماب")

TP_Tab:CreateButton({
   Name = "الانتقال إلى المانشن (Mansion)",
   Callback = function()
      local p = game.Players.LocalPlayer
      if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
         p.Character.HumanoidRootPart.CFrame = CFrame.new(15, 30, -50)
      end
   end,
})

TP_Tab:CreateButton({
   Name = "الانتقال إلى منطقة الأسلحة / المحل",
   Callback = function()
      local p = game.Players.LocalPlayer
      if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
         p.Character.HumanoidRootPart.CFrame = CFrame.new(120, 10, 80)
      end
   end,
})

TP_Tab:CreateButton({
   Name = "الانتقال إلى الشارع الرئيسي",
   Callback = function()
      local p = game.Players.LocalPlayer
      if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
         p.Character.HumanoidRootPart.CFrame = CFrame.new(0, 5, 0)
      end
   end,
})

-- 5. تبويب اللاعب والتحكم (Player Tab)
local PlayerTab = Window:CreateTab("اللاعب", 4483362458)
PlayerTab:CreateSection("تعديل خصائص الشخصية")

PlayerTab:CreateSlider({
   Name = "السرعة (WalkSpeed)",
   Range = {16, 150},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "SpeedFlag",
   Callback = function(Value)
      Settings.WalkSpeed = Value
   end,
})

PlayerTab:CreateSlider({
   Name = "قوة القفز (JumpPower)",
   Range = {50, 200},
   Increment = 5,
   Suffix = "Power",
   CurrentValue = 50,
   Flag = "JumpFlag",
   Callback = function(Value)
      Settings.JumpPower = Value
   end,
})

-- الحلقة البرمجية لتشغيل المزايا بكفاءة
task.spawn(function()
   while task.wait(0.3) do
      pcall(function()
         local player = game.Players.LocalPlayer
         local char = player.Character
         if not char then return end
         local hrp = char:FindFirstChild("HumanoidRootPart")
         local hum = char:FindFirstChild("Humanoid")

         -- تطبيق السرعة والقفز
         if hum then
            hum.WalkSpeed = Settings.WalkSpeed
            hum.JumpPower = Settings.JumpPower
         end

         -- الفارم التلقائي
         if Settings.AutoFarmCash and hrp then
            for _, v in pairs(workspace:GetChildren()) do
               if v:IsA("Model") and (v.Name:lower():find("cash") or v.Name:lower():find("money") or v.Name:lower():find("atm")) then
                  local targetPart = v.PrimaryPart or v:FindFirstChildOfClass("BasePart")
                  if targetPart then
                     hrp.CFrame = targetPart.CFrame * CFrame.new(0, 3, 0)
                     for _, p in pairs(v:GetDescendants()) do
                        if p:IsA("ProximityPrompt") then
                           fireproximityprompt(p)
                        end
                     end
                     task.wait(0.2)
                  end
               end
            end
         end

         -- تجميع العناصر
         if Settings.AutoCollect and hrp then
            for _, item in pairs(workspace:GetChildren()) do
               if item:IsA("Part") or item:IsA("MeshPart") then
                  if item.Name:lower():find("drop") or item.Name:lower():find("bag") then
                     item.CFrame = hrp.CFrame
                  end
               end
            end
         end

         -- توسيع Hitbox المنافسين
         if Settings.HitboxSize > 2 then
            for _, otherPlayer in pairs(game.Players:GetPlayers()) do
               if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                  local rootPart = otherPlayer.Character.HumanoidRootPart
                  rootPart.Size = Vector3.new(Settings.HitboxSize, Settings.HitboxSize, Settings.HitboxSize)
                  rootPart.Transparency = 0.7
                  rootPart.CanCollide = false
               end
            end
         end
      end)
   end
end)

print("--------------------------------------")
print("BlockSpin Script Hub Loaded Successfully!")
print("Developed by: dev night")
print("--------------------------------------")
