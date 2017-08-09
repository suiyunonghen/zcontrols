# zControls #
zControls is a collection of Delphi components,actually it contains only one component **TzObjectInspector**.    
forked from https://github.com/MahdiSafsafi/zcontrols

## 增加的特性 ##
  ###1、增加自定义属性名称###
  ```
  //OnGetPropNameEvent事件，可以指定自己想要的属性名称，比如
  procedure TMain.zObjectInspector1GetPropNameEvent(Sender: TControl;
  Item: PPropItem; var PropName: string);
  begin
    if PropName = 'Name' then
     PropName := '名称';
  end;
  //增加属性特性，可以直接给属性增加特性，以自动显示属性对应的其他命名，如
  type
  TSpeedButton = class(Vcl.Buttons.TSpeedButton)
  published
    [TPropAttribute('按钮名称','')]       //use CustomPropertryName and Add PropDescription
    property Name;
    [TPropAttribute('对齐方式','指定按钮的对齐方式')]
    property Align;
  end;
  最终，在TzObjectInspector中就会将Name属性名显示为 "按钮名称"，将 Align显示为“对齐方式"
  ```
  ###2、增加对于属性编辑执行是下拉列表还是对话框的事件判定处理###
  ```
  //OnCheckIfListProp
  //OnCheckIfDialogProp
  procedure TMain.zObjectInspector1CheckIfDialogProp(Sender: TControl;
  Item: PPropItem; Instance: TObject; var Handled, Result: Boolean);
  begin
    if Item.Prop.PropertyType.TypeKind = tkMethod then
    begin
      Handled := True;
      Result := True;
    end;
  end;

procedure TMain.zObjectInspector1CheckIfListProp(Sender: TControl;
  Item: PPropItem; Instance: TObject; var Handled, Result: Boolean);
begin
  if Item.Prop.PropertyType.TypeKind = tkMethod then
  begin
    Handled := True;
    Result := False;
  end;
  //这个代码将会将事件属性显示为对话框执行属性
end;
  ```
  ###3、增加对于属性下拉内容的自定义处理###
  ```
  //OnGetListItems
  procedure TMain.zObjectInspector1GetListItems(Sender: TControl; Item: PPropItem;
  Instance: TObject; Items: TStrings);
  begin
    if Item.Prop.PropertyType.Handle = TypeInfo(Boolean) then
    begin
      Items.AddObject('假', TObject(0));
      Items.AddObject('真', TObject(1));
   end
    else zObjectInspector1.DefaultGetPropListItems(Item,Items);
  end;
  ```
  ###3、增加对话框属性的自定义执行事件###
  ```
  //OnDialogPropExecute
procedure TMain.zObjectInspector1DialogPropExecute(Sender: TControl;
Item: PPropItem; Instance: TObject);
begin
  if Item.Prop.PropertyType.TypeKind = tkMethod then
     ShowMessage('事件属性')
  else zObjectInspector1.ExecuteDefaultDialog(Item);
end;
  ```
