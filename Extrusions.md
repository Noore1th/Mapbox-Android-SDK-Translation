---
 标题：“挤压”
 描述：“了解如何使用Android版Mapbox Maps SDK在地图上添加和自定义3D形状。”
 前置
   -“从'../../../components/context-dependent/android-activity-toggle'导入AndroidActivityToggle'；”
   -“从'@ mapbox / dr-ui / note'导入注释；”
   -“从'@ mapbox / dr-ui / related-page'导入RelatedPage”；”
 contentType：指南
 语言：
 -Java
 -科特林
 ---

 Mapbox样式规范使用术语*挤压*来表示地图上显示的3D形状。 挤压物经常以建筑物的形式出现在地图上，但是任何类型的“多边形”形状都可以挤压。 建筑物凸出部分的形状遵循建筑物的足迹形状，就好像二维“多边形”已从地图表面垂直拉伸了指定距离（以米为单位）。

 像Mapbox地图的许多其他视觉方面一样，可以在运行时并根据源数据中数据字段的值来设置拉伸样式。

 可以通过适用于Android的“ FillExtrusionLayer”的Maps SDK添加和设置拉伸样式。 与其他Maps SDK图层类似，“ FillExtrusionLayer”使用唯一的ID和唯一或共享的源ID进行初始化。 然后定义“ FillExtrusionLayer”图层的属性，然后将该图层添加到地图的“ Style”对象。

 ##样式

 适用于Android的Maps SDK拉伸样式选项遵循[官方Mapbox样式规范]（https://docs.mapbox.com/mapbox-gl-js/style-spec/layers/#fill-extrusion）。

 请阅读样式规范属性，以了解有关拉伸颜色，不透明度，图案，坐标偏移等的更多信息。

 [Mapbox Android演示应用程序的与拉伸有关的示例]（/ android / maps / examples /＃extrusions）展示了对拉伸进行样式设置的可能性。

 {{
 <AndroidActivityToggle
   id =“ basic-extrusion-setup”

 java = {`
 mapboxMap.getStyle（new Style.OnStyleLoaded（）{
 @Override
 public void onStyleLoaded（@NonNull样式样式）{

 FillExtrusionLayer fillExtrusionLayer = new FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）;

 fillExtrusionLayer.setProperties（
 fillExtrusionHeight（HEIGHT_NUMBER）
 ）;

 style.addLayer（fillExtrusionLayer）;
 }
 }）;
 }

 kotlin = {`
 mapboxMap.getStyle {

 val fillExtrusionLayer = FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）

 fillExtrusionLayer.setProperties（
 fillExtrusionHeight（HEIGHT_NUMBER）
 ）

 it.addLayer（fillExtrusionLayer）
 }
 }
 />
 }}

 ##光

 Mapbox样式规范的一部分是_light_的概念。 您可以调整光线照射到地图上的颜色，强度和角度，就像调整太阳的位置一样。

 样式的light属性为整个样式提供_global_光源。 因为这些属性应用于所有图层，所以“光”是样式规范中的[root属性]（https://docs.mapbox.com/mapbox-gl-js/style-spec/root/#light），而不是 在单个[填充-挤出层]（https://docs.mapbox.com/mapbox-gl-js/style-spec/layers/#fill-extrusion）中定义的绘画或布局属性。


 {{<Note>}}
 考虑使用Mapbox Studio迭代地图样式的灯光选项。 与在Android项目中进行迭代相比，在Mapbox Studio中工作可以使您更快地探索样式的灯光属性更改的视觉影响。 在Mapbox Studio中找到可创建所需拉伸样式的值之后，请在Android项目的代码中使用相同的最终值。

 [了解有关球坐标系的更多信息]（https://en.wikipedia.org/wiki/Spherical_coordinate_system）可以了解光线的径向，方位角和极坐标定位。
 {{</ Note>}}

 {{
   <相关页面
     url =“ / android / maps / examples / adjust-light-location-and-color /”
     title =“调光”
     contentType =“ example”>
 }}
 调整光源的位置和颜色，以查看其如何影响拉伸。
 {{</ RelatedPage>}}

 ##技巧

 通过Maps SDK的[表达式]（/ android / maps / overview / expressions /）和[数据驱动的样式]（/ android / maps / overview /数据驱动的样式/）可以增强挤出样式。 它们的组合可以带来有趣的效果，并实现特定的数据可视化以匹配您的用例。

 ###相对高度

 如果将挤出作为与空间中对象的物理高度无关的数据的数据可视化策略（例如，可视化人口普查块组中的种群），则数据字段的值可能不适合作为挤出 高度（以米为单位）。 要使突出显示在各种缩放级别下可见，您可以使用Maps SDK表达式使所有突出显示在地图上的高度更高**，并保持它们的相对高度差相同。 这样，当地图相机距离地图表面较远时，可以在低得多的缩放级别上看到突出部分。

 “ fillExtrusionHeight（sum（literal（CONSTANT_NUMBER），get（“ HEIGHT_FEATURE_PROPERTY_KEY”））））））;`是达到此效果的关键。

 {{
 <AndroidActivityToggle
   id =“ extrusion-height”

 java = {`
 mapboxMap.getStyle（new Style.OnStyleLoaded（）{
 @Override
 public void onStyleLoaded（@NonNull样式样式）{

 FillExtrusionLayer fillExtrusionLayer = new FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）;

 fillExtrusionLayer.setProperties（
 fillExtrusionColor（COLOR.RED），
 fillExtrusionHeight（sum（literal（CONSTANT_NUMBER），get（“ HEIGHT_FEATURE_PROPERTY_KEY”））））
 ）;

 style.addLayer（fillExtrusionLayer）;
 }
 }）;
 }

 kotlin = {`
 mapboxMap.getStyle {
 val fillExtrusionLayer = FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）

 fillExtrusionLayer.setProperties（
 fillExtrusionColor（Color.RED），
 fillExtrusionHeight（sum（literal（CONSTANT_NUMBER），get（“ HEIGHT_FEATURE_PROPERTY_KEY”））））
 ）

 it.addLayer（fillExtrusionLayer）
 }
 }

 />
 }}

 {{
   <相关页面
     url =“ / android / maps / examples / display-3d-building-height-based-on-vector-data /”
     title =“矢量数据高度”
     contentType =“ example”>
 }}
 根据数据集设置拉伸高度。
 {{</ RelatedPage>}}

 ###基础与高度

 调整fillExtrusionBase和fillExtrusionHeight之间的差异也会产生有趣的视觉效果。 如果差异很小，例如3米，则视觉效果将是看起来像漂浮在空中的一小部分挤压物。 这是可视化飞行数据或未连接到地图平坦表面的其他数据的一种不错而基本的方法。

 {{
 <AndroidActivityToggle
   id =“ base-vs-height”

 java = {`
 mapboxMap.getStyle（new Style.OnStyleLoaded（）{
 @Override
 public void onStyleLoaded（@NonNull样式样式）{

 FillExtrusionLayer fillExtrusionLayer = new FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）;

 fillExtrusionLayer.setProperties（
 fillExtrusionColor（Color.RED），
 fillExtrusionBase（get（“ HEIGHT_FEATURE_PROPERTY_KEY”）），
 fillExtrusionHeight（sum（literal（CONSTANT_NUMBER），get（“ HEIGHT_FEATURE_PROPERTY_KEY”））））
 ）;

 style.addLayer（fillExtrusionLayer）;
 }
 }）;

 }

 kotlin = {`
 mapboxMap.getStyle {

 val fillExtrusionLayer = FillExtrusionLayer（“ extrusion-layer-id”，“ source-id”）

 fillExtrusionLayer.setProperties（
 fillExtrusionColor（Color.RED），
 fillExtrusionBase（get（“ HEIGHT_FEATURE_PROPERTY_KEY”）），
 fillExtrusionHeight（sum（literal（CONSTANT_NUMBER），get（“ HEIGHT_FEATURE_PROPERTY_KEY”））））
 ）

 it.addLayer（fillExtrusionLayer）
 }
 }

 />
 }}
