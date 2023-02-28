# WinIoTTestsAndStudy
Test ptojects while studt



<Capabilities>
    <Capability Name="internetClient" />
	  <DeviceCapability Name="serialcommunication">
		  <Device Id="any">
			  <Function Type="name:serialPort"/>
		  </Device>
	  </DeviceCapability>
  </Capabilities>


          <TextBox x:Name="edtLink" TextWrapping="Wrap" Text="https://www.youtube.com/watch?v=O_g6vrItpAY" VerticalAlignment="Top" FontSize="20" TextChanged="edtLink_TextChanged"/>
        <Button x:Name="btnOpen" Content="Abrir" Margin="695,185,0,0" VerticalAlignment="Top" Click="btnOpen_Click"/>
        <Button x:Name="btnClose" Content="Fechar" Margin="762,185,0,0" VerticalAlignment="Top" Click="btnClose_Click"/>
        <TextBox x:Name="edtTextToSend" HorizontalAlignment="Left" Height="30" Margin="511,244,0,0" TextWrapping="Wrap" Text="TextBox" VerticalAlignment="Top" Width="235"/>
        <Button x:Name="btnSend" Content="Enviar" Margin="767,244,0,0" VerticalAlignment="Top" Click="btnSend_Click"/>
        <ComboBox x:Name="cbDeviceList" Width="314" Margin="511,102,0,0" SelectionChanged="cbDeviceList_SelectionChanged"/>
        <Button x:Name="btnGetDevices" Content="Carregar Lista" Margin="511,65,0,0" VerticalAlignment="Top" Click="Button_Click"/>
        <TextBox x:Name="edtPort" HorizontalAlignment="Left" Margin="511,185,0,0" TextWrapping="Wrap" Text="TextBox" VerticalAlignment="Top" Width="157"/>
        <Button x:Name="btnExit" Content="Sair" Margin="30" VerticalAlignment="Bottom" Width="89" Height="61" Click="btnExit_Click" HorizontalAlignment="Right"/>


        private void btnSend_Click(object sender, RoutedEventArgs e)
        {
              if(serialPort != null)
                {
                    if(serialPort.IsOpen)
                    {
                        serialPort.Open();
                        serialPort.Write(edtTextToSend.Text);
                    }
              }
        }

        private async void Button_Click(object sender, RoutedEventArgs e)
        {
            cbDeviceList.Items.Clear();

            IAsyncOperation<DeviceInformationCollection> asyncOperation = DeviceInformation.FindAllAsync();
            (await asyncOperation).ToList().ForEach(r =>
                cbDeviceList.Items.Add(r.Name)
            );

        }

        private void btnOpen_Click(object sender, RoutedEventArgs e)
        {
            if (serialPort != null)
            {
                if (serialPort.IsOpen)
                {
                    serialPort.Close();
                }
            }

            serialPort = new SerialPort(edtPort.Text, 9600);
            serialPort.Open();
        }

        private void cbDeviceList_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            edtPort.Text = cbDeviceList.SelectedItem.ToString();
        }

        private void btnClose_Click(object sender, RoutedEventArgs e)
        {
            if (serialPort != null)
            {
                if (serialPort.IsOpen)
                {
                    serialPort.Close();
                }
                serialPort = null;
            }

        }

        private void btnExit_Click(object sender, RoutedEventArgs e)
        {
            Application.Current.Exit();
        }
 