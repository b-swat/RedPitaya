#######################
Streaming
#######################

Streaming application enables user to stream data from Red Pitaya to :

    * Local file stored on Red Pitaya SD card
    * Over ethernet to remote computer using UDP or TCP protocol

User is able to set:

    * sampling frequency
    * number of input channels
    * input channel resolution

Streamed data can be stored into:

    * Standard audio WAV file format
    * Technical Data Management Streaming (TDMS) file format

Max. streaming speeds are limited to:

    * 10MB/s for streaming to SD card (SD card class 10 recommended for best streaming performance)
    * 20MB/s for streaming over 1Gbit network (`direct ethernet connection is recommended to achieve best streaming performance <https://redpitaya.readthedocs.io/en/latest/quickStart/connect/connect.html#direct-ethernet-cable-connection>`__.)


**********************************************
Start using Red Pitaya streaming feature
**********************************************

#. ) Run streaming app from Red Pitaya WEB interface

    .. image:: img/redpitaya_main_page.png

#. ) Stream locally to a file

    #. ) Set app properties & click RUN
    
        .. image:: img/to_file_settings.png
           :width: 60%
    
        Example: streaming on ch1, 8bit resolution 10Msps into TDMS file format
    
    #. ) Press STOP to stop streaming

    #. ) Click Browse to open file browser and download streaming data file

        .. image:: img/capture.png
           :width: 80%

    #. ) Open file in `DIAdem software <https://www.ni.com/en-us/shop/data-acquisition-and-control/application-software-for-data-acquisition-and-control-category/what-is-diadem.html>`__
         that supports TDMS file reading, visualization & processing.

        .. image:: img/diadem_tdms_file_viewer.png
           :width: 80%

#. ) Streaming to remote computer

    #. ) Download streaming client to your computer.

         `Linux <http://downloads.redpitaya.com/downloads/Clients/streaming/linux-tool.zip>`__.
         `Windows <http://downloads.redpitaya.com/downloads/Clients/streaming/windows-tool.zip>`__.

    #. ) Set app properties & click RUN

        .. image:: img/tcp_settings.png
           :width: 60%
        
        Example: streaming on ch1, 16bit resolution 5Msps, TCP
    
    #. ) Run streaming app on remote computer (copy IP from the WEB interface and select required file format)

        .. code-block:: console

           rpsa_client.exe -h 169.254.43.84 -p TCP -f ./ -t wav

        .. image:: img/tcp_client.png
           :width: 60%

        Data streaming can be stopped by pressing Ctrl + C

    #. ) Created wav file can be read or visualized using `Audacity software <https://www.audacityteam.org/>`__:

        .. image:: img/audacity.png
           :width: 80%


*******************
Console application
*******************

The server for streaming can be started not only using the web interface, but also through the command line.

.. code-block:: console

    root@rp-f07167:/# streaming-server 
    Missing parameters: Configuration file
    Usage: streaming-server
	    -b run service in background
	    -c path to config file

To start the server, you need to do 3 steps:

    #. ) Load the FPGA image of streaming

        .. code-block:: console

            root@rp-f07167:/# cat /opt/redpitaya/fpga/fpga_streaming.bit > /dev/xdevcfg 

    #. ) Prepare a configuration file.

    #. ) Launch a console application.

        .. code-block:: console

            root@rp-f07167:/# streaming-server -c /root/.streaming_config 
            streaming-server started
            Lost rate: 0 / 763 (0 %)
            Lost rate: 0 / 766 (0 %)
            Lost rate: 0 / 766 (0 %)
            Lost rate: 0 / 766 (0 %)

The configuration for streaming is automatically created and saved in the file: **/root/.streaming_config** during editing the parameters in the web application.


.. note::

    Any changes to the web application will automatically modify the configuration file. If you want to save the configuration, then make a copy of the file.

.. note::

    The server can be started in the background. To do this, use the -b parameter. In this mode, the application can be used as a service at system startup. Service information from the application is saved in the syslog file (by default, the syslog is not installed on RP).

.. note::

    Streaming always creates two files:
        * first stores streamed data
        * second data transfer report

.. note::

    Streaming app source are available here: `streaming app <https://github.com/RedPitaya/RedPitaya/tree/master/apps-tools/streaming_manager>`__.
        