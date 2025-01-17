在OpenCV中，使用PnP（Perspective-n-Point）解算求解相机姿态时，平移矩阵（Translation Matrix）是使用相机坐标系（Camera Coordinate System）表示的。

相机坐标系是一个右手坐标系，原点位于相机的光学中心，X轴指向图像的右侧，Y轴指向图像的下方，Z轴指向相机的光轴方向（从相机的光学中心向外延伸）。

当使用PnP解算得到相机的平移向量时，平移向量表示从相机坐标系原点（光学中心）到世界坐标系原点（或其他参考点）的平移向量，以相机坐标系为参考。平移向量的单位通常是与世界坐标系一致的长度单位，例如米（m）或毫米（mm）。

需要注意的是，PnP解算中的旋转矩阵和旋转向量也是使用相机坐标系表示的，并遵循右手坐标系约定。因此，当使用PnP解算求解相机姿态时，所有的姿态参数（平移矩阵、旋转矩阵、旋转向量）都是在相机坐标系中表示的。

PnP解算得到的平移矩阵和旋转矩阵表示了==相机从世界坐标系到相机坐标系的变换==。