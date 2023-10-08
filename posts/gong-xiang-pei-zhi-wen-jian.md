<!--
.. title: windows-intellij-keymap
.. slug: gong-xiang-pei-zhi-wen-jian
.. date: 2023-08-13 19:10:56 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

JetBrains/IntelliJIdea2022.1/keymaps

```
<keymap version="1" name="Emacs copy" parent="Emacs">
  <action id="AceAction">
    <keyboard-shortcut first-keystroke="alt enter" />
  </action>
  <action id="ActivateTerminalToolWindow">
    <keyboard-shortcut first-keystroke="alt f12" />
    <keyboard-shortcut first-keystroke="alt j" />
  </action>
  <action id="Adtui.ZoomInAction">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="Adtui.ZoomOutAction">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="Back">
    <keyboard-shortcut first-keystroke="ctrl alt left" />
    <mouse-shortcut keystroke="button4" />
    <keyboard-shortcut first-keystroke="ctrl minus" />
  </action>
  <action id="CloseGotItTooltip">
    <keyboard-shortcut first-keystroke="escape" />
    <keyboard-shortcut first-keystroke="ctrl g" />
  </action>
  <action id="CollapseAll">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="CollapseExpandableComponent">
    <keyboard-shortcut first-keystroke="shift enter" />
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="CollapseRegion">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="CollapseSelection">
    <keyboard-shortcut first-keystroke="ctrl period" />
    <keyboard-shortcut first-keystroke="alt period" />
  </action>
  <action id="Console.Execute.Multiline" />
  <action id="Console.TableResult.EditValue">
    <keyboard-shortcut first-keystroke="f2" />
    <keyboard-shortcut first-keystroke="enter" />
  </action>
  <action id="Console.TableResult.NavigateForeignAction">
    <keyboard-shortcut first-keystroke="ctrl alt g" />
    <keyboard-shortcut first-keystroke="escape" second-keystroke="period" />
    <mouse-shortcut keystroke="control button1" />
    <keyboard-shortcut first-keystroke="alt minus" />
    <mouse-shortcut keystroke="alt button1" />
  </action>
  <action id="DSM.ZoomIn">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="DatabaseView.Tools" />
  <action id="DirDiffMenu.SynchronizeDiff.All" />
  <action id="Docker.RemoteServers.StartComposeService" />
  <action id="Docker.RemoteServers.StartContainer" />
  <action id="EditorCutLineBackward" />
  <action id="EditorToggleStickySelection">
    <keyboard-shortcut first-keystroke="ctrl semicolon" />
  </action>
  <action id="ExpandAll">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="ExpandExpandableComponent">
    <keyboard-shortcut first-keystroke="shift enter" />
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="ExpandRegion">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="ExternalSystem.CollapseAll">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="ExternalSystem.ExpandAll">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="ExternalSystem.ProjectRefreshAction" />
  <action id="FileStructurePopup">
    <keyboard-shortcut first-keystroke="ctrl f12" />
    <keyboard-shortcut first-keystroke="shift alt o" />
  </action>
  <action id="FindInPath">
    <keyboard-shortcut first-keystroke="shift ctrl f" />
    <keyboard-shortcut first-keystroke="shift alt f" />
  </action>
  <action id="FindNext">
    <keyboard-shortcut first-keystroke="f3" />
  </action>
  <action id="FocusEditor">
    <keyboard-shortcut first-keystroke="escape" />
    <keyboard-shortcut first-keystroke="ctrl g" />
  </action>
  <action id="ForceOthersToFollowAction" />
  <action id="Forward">
    <keyboard-shortcut first-keystroke="ctrl alt right" />
    <mouse-shortcut keystroke="button5" />
    <keyboard-shortcut first-keystroke="ctrl equals" />
  </action>
  <action id="Generate">
    <keyboard-shortcut first-keystroke="alt g" />
  </action>
  <action id="GotoAction">
    <keyboard-shortcut first-keystroke="escape" second-keystroke="x" />
    <keyboard-shortcut first-keystroke="alt x" />
    <keyboard-shortcut first-keystroke="alt x" />
  </action>
  <action id="GotoDeclaration">
    <keyboard-shortcut first-keystroke="ctrl alt g" />
    <keyboard-shortcut first-keystroke="escape" second-keystroke="period" />
    <mouse-shortcut keystroke="control button1" />
    <keyboard-shortcut first-keystroke="alt minus" />
    <mouse-shortcut keystroke="alt button1" />
  </action>
  <action id="GotoImplementation">
    <keyboard-shortcut first-keystroke="alt equals" />
    <mouse-shortcut keystroke="alt button1" />
  </action>
  <action id="GotoTypeDeclaration">
    <keyboard-shortcut first-keystroke="shift ctrl b" />
    <mouse-shortcut keystroke="shift control button1" />
    <keyboard-shortcut first-keystroke="alt 0" />
  </action>
  <action id="Graph.ActualSize">
    <keyboard-shortcut first-keystroke="ctrl divide" />
  </action>
  <action id="Images.Editor.ActualSize">
    <keyboard-shortcut first-keystroke="ctrl divide" />
  </action>
  <action id="Images.Editor.ZoomIn">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="Images.Editor.ZoomOut">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="KotlinShellExecute" />
  <action id="Maven.CollapseAll">
    <keyboard-shortcut first-keystroke="ctrl subtract" />
  </action>
  <action id="Maven.ExpandAll">
    <keyboard-shortcut first-keystroke="ctrl add" />
  </action>
  <action id="OptimizeImports">
    <keyboard-shortcut first-keystroke="ctrl alt o" />
    <keyboard-shortcut first-keystroke="shift ctrl o" />
  </action>
  <action id="Refactorings.QuickListPopupAction">
    <keyboard-shortcut first-keystroke="shift ctrl alt t" />
    <keyboard-shortcut first-keystroke="alt s" />
  </action>
  <action id="ReformatCode">
    <keyboard-shortcut first-keystroke="shift alt period" />
  </action>
  <action id="SelectNextOccurrence" />
  <action id="SetShortcutAction" />
  <action id="ShowIntentionActions">
    <keyboard-shortcut first-keystroke="alt space" />
    <keyboard-shortcut first-keystroke="ctrl enter" />
  </action>
  <action id="SplitChooser.Duplicate" />
  <action id="StepOver">
    <keyboard-shortcut first-keystroke="f8" />
    <keyboard-shortcut first-keystroke="alt n" />
  </action>
  <action id="Terminal.SmartCommandExecution.Run" />
  <action id="Terminal.SwitchFocusToEditor">
    <keyboard-shortcut first-keystroke="escape" />
    <keyboard-shortcut first-keystroke="ctrl g" />
  </action>
  <action id="ToggleBookmark">
    <keyboard-shortcut first-keystroke="f11" />
    <keyboard-shortcut first-keystroke="alt b" />
  </action>
  <action id="ToggleBookmarkWithMnemonic">
    <keyboard-shortcut first-keystroke="ctrl f11" />
    <keyboard-shortcut first-keystroke="shift alt b" />
  </action>
  <action id="Uml.NodeIntentions" />
  <action id="ViewSource" />
</keymap>
```
